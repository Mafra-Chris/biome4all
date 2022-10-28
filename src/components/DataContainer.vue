<script setup lang="ts">
import LevelChart from '../components/LevelChart.vue';
import DoughnutChart from '../components/DoughnutChart.vue';

const props = defineProps({
  activeSample: { type: Object, required: true },
});
</script>

<template>
  <div>
    <div class="font-semibold">
      <span class="text-black mr-2 text-lg font-bold">Amostra </span>
      <h1
        class="bg-biome-100 inline-block px-3 py-1 rounded-tl-xl rounded-br-xl text-white mb-3 md:mb-4"
      >
        {{ activeSample.name }}
      </h1>
    </div>
    <div
      class="grid grid-cols-1 lg:grid-cols-2 gap-2 xl:gap-6 bg-biome-light rounded-lg shadow-lg p-3 bg-opacity-80"
    >
      <div v-for="chart in activeSample.levelChart" :key="chart.name">
        <LevelChart :level="chart.value" :name="chart.name"></LevelChart>
      </div>
    </div>
    <div class="font-semibold mt-6">
      <h1
        class="bg-biome-100 inline-block px-3 py-1 rounded-tl-xl rounded-br-xl text-white mb-3 md:mb-10"
      >
        Bactérias
      </h1>
    </div>
    <DoughnutChart
      class="text-center <xs:-mt-24 sm:-mt-6 bg-biome-light rounded-lg shadow-lg p-3 bg-opacity-80"
      :values="activeSample.doughnutChart"
    ></DoughnutChart>
  </div>
</template>
