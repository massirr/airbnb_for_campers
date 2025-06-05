<script>
import L from 'leaflet';
import 'leaflet/dist/leaflet.css'; // Needed to display the map tiles correctly
import locationPin from '@/assets/image/location-pin.png';

export default {
  name: 'MapComp',
  props: {
    location: {
      type: Object
    },
  },
  data() {
    return {
      map: null,     // Holds the Leaflet map instance
      camps: null     // Will hold the fetched camp info from the backend
    };
  },
  mounted() {
    this.initMap(); // Initialize the map first

    if (this.location) { // from the prop passed from campsinfopage
      const lat = parseFloat(this.location.latitude);
      const lon = parseFloat(this.location.longitude);

      const customIcon = L.icon({
        iconUrl: locationPin,
        iconSize: [32, 32],
        iconAnchor: [16, 32],
        popupAnchor: [0, -32]
      });

      // Update the map's center to the prop's coordinates
      this.map.setView([lat, lon], 13);

      // Add a marker for the prop's coordinates --> [lat, lon] - ensures the marker is placed at the correct latitude and longitude
      L.marker([lat, lon], { icon: customIcon })
        .addTo(this.map)
        .bindPopup(this.location.name) // the name of the location
        .openPopup();
    } else {
      this.getCamps(); // Fetch the camps data and add markers
    }
  },
  methods: {
    // Sets up the map view with default coordinates and adds the tile layer
    initMap() {
      // L.map() Referring to the ID of the HTML element where the map will be rendered.
      this.map = L.map('map').setView([50.85, 4.3517], 13); // Default coordinates [lat, lon] and zoom level

      // The tile layer makes the map visible
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; OpenStreetMap contributors'
      }).addTo(this.map);
    },

    // Fetches the coordinates from the backend and adds a marker with a popup
    getlatlong() {
      fetch("http://localhost:3000/latlon/?location=Mechelen, Belgium", { 
        method: "GET"
      })
        .then((response) => response.json())
        .then((data) => {

          const lat = parseFloat(data.lat);
          const lon = parseFloat(data.lon);

          const customIcon = L.icon({ // shows the icon of locations
            iconUrl: locationPin,
            iconSize: [32, 32],
            iconAnchor: [16, 32],
            popupAnchor: [0, -32]
          });

          // Update the map's center to the marker's coordinates
          this.map.setView([lat, lon], 13);

          // The camping spot marker on the map with a popup
          L.marker([lat, lon], { icon: customIcon })
            .addTo(this.map)
            .bindPopup(`Camping Spot: ${lat}, ${lon}`) // Can be updated to show a location name or details
            .openPopup(); // Opens the popup immediately
        })
        .catch((error) => {
          console.error("Geocode fetch failed", error); // Log any fetch errors
        });
    },

    // fetch all the camp.name with their lat and lon
    getCamps() {
      fetch("http://localhost:3000/camps", {
        method: "GET"
      })
        .then((response) => response.json())
        .then((_camps) => {
          this.camps = _camps;
          //console.log(this.camps); // Log the camps data after it is fetched
          
          // Add markers for all camps
          const customIcon = L.icon({
            iconUrl: locationPin,
            iconSize: [32, 32],
            iconAnchor: [16, 32],
            popupAnchor: [0, -32]
          });
          
          // Prepare for bounds adjustment
          const markers = [];
          const bounds = L.latLngBounds();
          
          this.camps.forEach((camp) => {
            const lat = parseFloat(camp.latitude);
            const lon = parseFloat(camp.longitude);
            
            // Skip invalid coordinates
            if (isNaN(lat) || isNaN(lon)) {
              console.error("Invalid coordinates for camp:", camp);
              return;
            }
            
            //console.log(`Adding marker at [${lat}, ${lon}] for "${camp.name}"`);
            
            const marker = L.marker([lat, lon], { icon: customIcon })
              .addTo(this.map)
              .bindPopup(camp.name); // Display the camp name in the popup
            
            markers.push(marker);
            bounds.extend([lat, lon]);
          });
          
          // Adjust map view to show all markers
          if (markers.length > 0) {
            this.map.invalidateSize(); // Force recalculation of map size
            this.map.fitBounds(bounds, { padding: [50, 50] });
          }
        })
        .catch((error) => {
          console.error("Error fetching camps:", error); // Handle errors
        });
    },

  }
};
</script>

<template>
  <div>
    <!-- This is where the map will be displayed -->
    <div id="map" class="h-[400px] w-full rounded-lg shadow-lg"></div>
  </div>
</template>
