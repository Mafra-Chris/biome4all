<template>
  <Doughnut
    :chart-options="chartOptions"
    :chart-data="chartData"
    :chart-id="chartId"
    :dataset-id-key="datasetIdKey"
    :css-classes="cssClasses"
    :styles="styles"
    :width="width"
    :height="height"
  />
</template>

<script lang="ts">
import { Doughnut } from 'vue-chartjs';
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  ArcElement,
  CategoryScale,
  Plugin,
} from 'chart.js';

ChartJS.register(Title, Tooltip, Legend, ArcElement, CategoryScale);

export default {
  name: 'DoughnutChart',
  components: { Doughnut },
  props: {
    chartId: {
      type: String,
      default: 'doughnut-chart',
    },
    datasetIdKey: {
      type: String,
      default: 'label',
    },
    width: {
      type: Number,
      default: 400,
    },
    height: {
      type: Number,
      default: 400,
    },
    cssClasses: {
      default: '',
      type: String,
    },
    styles: {
      type: Object,
      default: () => {},
    },
    plugins: {
      type: Object,
      default: () => {},
    },
    values: {
      type: Array<number>,
      required: true,
    },
  },
  data() {
    return {
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
          legend: {
            position: 'right',
          },
        },
      },
    };
  },
  computed: {
    chartData(): any {
      return {
        labels: [
          'Proteobacteria',
          'Actinobacteria',
          'Acidobacteria',
          'Bacteroides',
          'Outros',
        ],
        datasets: [
          {
            backgroundColor: [
              '#90A955',
              '#738844',
              '#639438',
              '#4F772D',
              '#31572C',
            ],
            data: this.values,
          },
        ],
      };
    },
  },
};
</script>
