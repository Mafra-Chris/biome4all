<script setup lang="ts">
import LevelChart from '../components/LevelChart.vue';
import DoughnutChart from '../components/DoughnutChart.vue';
import axios from 'axios';

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
      };
    });

    return samples;
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
  },
]);
const loading = ref(true);

onMounted(async () => {
  allSamples.value = await getAllSamples(SAMPLES_URLs);
  activeSample.value = allSamples.value[0];
  loading.value = false;
});
</script>

<template>
  <div v-if="!loading">
    <div>
      <h1
        class="bg-biome-100 inline-block px-3 py-1 rounded-tl-xl rounded-br-xl text-white font-semibold mb-8"
      >
        {{ activeSample.name }}
      </h1>
    </div>
    <div class="grid grid-cols-1 md:grid-cols-2">
      <div v-for="chart in activeSample.levelChart" :key="chart.name">
        <LevelChart :level="chart.value" :name="chart.name"></LevelChart>
      </div>
      <DoughnutChart
        class="text-center md:col-span-2 mt-10"
        :values="activeSample.doughnutChart"
      ></DoughnutChart>
    </div>
  </div>
</template>
