<template>
  <div id="map" class="h-96"></div>
</template>

<script setup lang="ts">
import leaflet from 'leaflet';

let mymap;
onMounted(() => {
  mymap = leaflet.map('map').setView([51.505, -0.09], 13);

  leaflet
    .tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 19,
      attribution:
        '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
    })
    .addTo(mymap);
});
</script>
