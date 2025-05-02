<script>
import L from 'leaflet';
import 'leaflet/dist/leaflet.css'; // Needed to display the map tiles correctly

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
      // Referring to the ID of the HTML element where the map will be rendered.
      const map = L.map('map').setView([50.85, 4.3517], 13); // Default coordinates [lat, lon] and zoom level

      // The tile layer makes the map actually visible
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; OpenStreetMap contributors'
      }).addTo(map);
    },

    // Fetches the coordinates from the backend and adds a marker with a popup
    getlatlong() {
      fetch("http://localhost:3000/camps", {
        method: "GET"
      })
        .then((response) => response.json())
        .then((data) => {
          this.code = data; // Store the lat/lon in component state

          const lat = parseFloat(data.lat);
          const lon = parseFloat(data.lon);

          const customIcon = L.divIcon({
            html: '<i class="pi pi-map-marker text-2xl text-red-500"></i>', // PrimeIcon class
            className: 'leaflet-div-icon', // Remove default styling
            iconSize: [30, 30],
            iconAnchor: [15, 30],
            popupAnchor: [0, -30]
          });

          // The camping spot marker on the map with a popup
          L.marker([50.8503, 4.3517], { icon: customIcon })
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

    <!-- Displaying fetched coordinates for debug or reference -->
    <div class="text-black text-center mt-2">
      Coordinates: {{ code?.lat }}, {{ code?.lon }}
      map: {{ map }}
    </div>
  </div>
</template>
