<template>
  <div id="map" class="h-96 w-full rounded-lg shadow-lg"></div>
</template>

<script>
import L from 'leaflet';

export default {
  name: 'MapComp',
  mounted() {
    const map = L.map('map').setView([51.505, -0.09], 13); // Default coordinates

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '&copy; OpenStreetMap contributors'
    }).addTo(map);

    L.marker([51.505, -0.09]).addTo(map)
      .bindPopup('Camping Spot')
      .openPopup();
  }
};
</script>

<style scoped>
#map {
  height: 400px;
}
</style>
