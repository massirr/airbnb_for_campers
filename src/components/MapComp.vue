<script>
  import L from 'leaflet';
  
  export default {
    name: 'MapComp',
    mounted() {
      // referring to the ID of the HTML element where the map will be rendered.
      const map = L.map('map').setView([50.85, 4.3517], 13); // Default coordinates [long, lat] zoom
  
      // the line that makes the map actually visible
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; OpenStreetMap contributors'
      }).addTo(map);
      
      // the camping spots.
      L.marker([50.8503, 4.3517]).addTo(map)
        .bindPopup('Camping Spot') // To be replaced with a location icon
        .openPopup();
    }
  };
</script>

<template>
  <div id="map" class="h-[400px] w-full rounded-lg shadow-lg"></div>
</template>
