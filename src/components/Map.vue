<template>
  <div id="map" class="h-80 md:h-full"></div>
</template>

<script setup lang="ts">
import leaflet from 'leaflet';

const props = defineProps({
  geoJson: { type: Array, required: true },
  activeName: { type: String, required: true },
});
let mymap: any;
let areas: any;
let activeMap = ref('');
const emit = defineEmits(['setActive']);
function setActive(name: string) {
  areas.forEach((area) => {
    if (area.name === name) {
      area.marker._icon.classList.remove('grey-hue');
      area.marker._icon.classList.add('green-hue');
    } else {
      area.marker._icon.classList.add('grey-hue');
    }
  });
  activeMap.value = name;
  emit('setActive', name);
}
onMounted(() => {
  activeMap.value = props.activeName;

  mymap = leaflet.map('map').setView([0, 0], 13);
  leaflet
    .tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 19,
      attribution:
        '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
    })
    .addTo(mymap);

  areas = props.geoJson.map((el: any) => {
    let coordinates = el.map.features[0].geometry.coordinates[0][0][0];
    let marker = leaflet
      .marker([coordinates[1], coordinates[0]])
      .addTo(mymap)
      .on('click', () => {
        setActive(el.name);
      });
    if (el.name === activeMap.value) {
      marker._icon.classList.add('green-hue');
    } else {
      marker._icon.classList.add('grey-hue');
    }
    return {
      polygon: leaflet
        .geoJSON(el.map, {
          color: 'grey',
          fillColor: 'grey',
          fillOpacity: 0.5,
        })
        .addTo(mymap)
        .on('click', () => {
          setActive(el.name);
        }),
      marker: marker,
      name: el.name,
    };
  });
  let coordinates =
    props.geoJson[0].map.features[0].geometry.coordinates[0][0][0];
  mymap.setView([coordinates[1], coordinates[0]], 9);
});
//90A955
</script>
<style>
img.green-hue {
  filter: invert(62%) sepia(54%) saturate(337%) hue-rotate(37deg)
    brightness(91%) contrast(91%);
}
img.grey-hue {
  filter: invert(49%) sepia(7%) saturate(14%) hue-rotate(314deg) brightness(93%)
    contrast(84%);
}
</style>
