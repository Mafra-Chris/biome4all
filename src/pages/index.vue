<script setup lang="ts">
import axios from 'axios';
import LevelChart from '../components/LevelChart.vue';
import DoughnutChart from '../components/DoughnutChart';

async function getSample(sampleName: String) {
  try {
    let response = await axios.get(
      `https://dev.b4adashboard.com/api/teste/${sampleName}`
    );
    console.log(response.data);
  } catch (error: any) {
    if (error.response.status === 404) {
      console.log('Amostra não encontrada!');
    } else {
      console.log(error);
    }
  }
}
onMounted(() => {
  getSample('teste1');
});
</script>

<template>
  <div class="text-center">
    <LevelChart :level="3" name="Ácido abscísico"></LevelChart>
    <DoughnutChart class="w-1/2" :values="[40, 20, 80, 10, 30]"></DoughnutChart>
  </div>
</template>

<style></style>
