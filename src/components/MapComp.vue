<script>
import L from 'leaflet';
import 'leaflet/dist/leaflet.css'; // Needed to display the map tiles correctly
import locationPin from '@/assets/image/location-pin.png';

export default {
  name: 'MapComp',
  data() {
    return {
      map: null,     // Holds the Leaflet map instance
      code: null     // Will hold the fetched coordinates from the backend
    };
  },
  mounted() {
    this.initMap();     // Initialize the map when the component mounts
    this.getlatlong();  // Fetch geocoded coordinates from the backend
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
      fetch("http://localhost:3000/latlon", { 
        method: "GET"
      })
        .then((response) => response.json())
        .then((data) => {
          this.code = data; // Store the lat/lon in component state

          const lat = parseFloat(data.lat);
          const lon = parseFloat(data.lon);

          const customIcon = L.icon({ // shows the icon of locations
            iconUrl: locationPin,
            iconSize: [32, 32],
            iconAnchor: [16, 32],
            popupAnchor: [0, -32]
          });

          // The camping spot marker on the map with a popup
          L.marker([lat, lon], { icon: customIcon })
            .addTo(this.map)
            .bindPopup(`Camping Spot: ${lat}, ${lon}`) // Can be updated to show a location name or details
            .openPopup(); // Opens the popup immediately
        })
        .catch((error) => {
          console.error("Geocode fetch failed", error); // Log any fetch errors
        });
    }
  }
};
</script>

<template>
  <div>
    <!-- This is where the map will be displayed -->
    <div id="map" class="h-[400px] w-full rounded-lg shadow-lg"></div>
  </div>
</template>
