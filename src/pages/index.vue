<script setup lang="ts">
import Map from '../components/Map.vue';
import DataContainer from '../components/DataContainer.vue';
import axios from 'axios';
import { SwappingSquaresSpinner } from 'epic-spinners';
const SAMPLES_URLs = [
  'https://dev.b4adashboard.com/api/teste/teste1',
  'https://dev.b4adashboard.com/api/teste/teste2',
  'https://dev.b4adashboard.com/api/teste/teste3',
  'https://dev.b4adashboard.com/api/teste/teste4',
];

async function getAllSamples(samplesURLs: string[]) {
  const promises = samplesURLs.map((url) => {
    return axios.get(url);
  });
  let responses: object[] = [];

  await Promise.all(promises)
    .then(function (values: any) {
      values.forEach((value: any) => {
        responses.push(value.data);
      });
    })
    .catch((error) => {
      if (error.response.status === 404) {
        console.log('Amostra não encontrada!');
      } else {
        console.log('Erro ao buscar amostras!');
      }
    });

  if (responses.length > 0) {
    let samples = responses.map((response: any) => {
      let entries = Object.entries(response.graficos2);
      let levelChart: Array<{ name: string; value: number }> = [];
      entries.map(([key, val]) => {
        levelChart.push({
          name: key,
          value: typeof val === 'number' ? val : 0,
        });
      });
      return {
        name: response.identificacao_amostra,
        levelChart: levelChart,
        doughnutChart: response.graficos1,
        map: response.mapa,
      };
    });

    return samples;
  }
}

function setActiveSample(value: string) {
  let found = allSamples.value.find((sample) => sample.name === value);
  if (found) {
    activeSample.value = found;
  }
}

const activeSample = ref({
  name: '',
  levelChart: [
    {
      name: '',
      value: 0,
    },
  ],
  doughnutChart: [0],
});
const allSamples = ref([
  {
    name: '',
    levelChart: [
      {
        name: '',
        value: 0,
      },
    ],
    doughnutChart: [0],
    map: {},
  },
]);
const allGeoJson: any = ref([]);
const loading = ref(true);

onMounted(async () => {
  allSamples.value = await getAllSamples(SAMPLES_URLs);
  allSamples.value.forEach((el) => {
    allGeoJson.value.push({ name: el.name, map: el.map });
  });
  activeSample.value = allSamples.value[0];
  loading.value = false;
});
</script>

<template>
  <div class="h-full flex flex-col z-0">
    <Header class="flex-grow-0 flex-shrink basis-auto mb-6"></Header>
    <div
      class="grid grid-cols-1 md:grid-cols-2 h-full flex-grow-1 flex-shrink basis-auto"
      v-if="!loading"
    >
      <div class="px-4 pb-4 lg:px-8 lg:pb-8">
        <Map
          :geoJson="allGeoJson"
          :activeName="activeSample.name"
          @setActive="setActiveSample"
          class="rounded-lg border-gray-400 border"
        ></Map>
      </div>
      <DataContainer
        :activeSample="activeSample"
        class="pb-8 pl-4 pr-4 md:pl-0 lg:pr-8"
      ></DataContainer>
    </div>
    <div
      v-else
      class="h-screen w-screen text-center flex justify-center items-center"
    >
      <swapping-squares-spinner
        :animation-duration="1000"
        :size="105"
        color="#90A955"
      />
    </div>
  </div>
</template>

<style>
body {
  min-height: 100vh;
}
#app {
  @apply h-screen;
}
</style>
