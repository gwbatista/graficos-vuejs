<template>
  <div class="chart-container">
    <h1>As 10 maiores variações em ordem decrescente</h1>
    <bar-chart v-if="datacollection && datacollection.labels" :data="datacollection"></bar-chart>
    <p v-else>Carregando dados...</p>
  </div>
</template>

<script>
import { Bar } from 'vue-chartjs';
import { Chart as ChartJS, CategoryScale, LinearScale, BarElement, Title, Tooltip, Legend } from 'chart.js';
import axios from 'axios';

ChartJS.register(CategoryScale, LinearScale, BarElement, Title, Tooltip, Legend);

export default {
  components: {
    'bar-chart': Bar
  },
  data() {
    return {
      datacollection: {
        labels: [],
        datasets: []
      }
    };
  },
  async mounted() {
    try {
      const response = await axios.get('https://economia.awesomeapi.com.br/json/daily/USD-BRL/30');
      const data = response.data;

      const sortedData = data.sort((a, b) => b.pctChange - a.pctChange).slice(0, 10);

      const labels = sortedData.map(item => new Date(item.timestamp * 1000).toLocaleDateString());
      const values = sortedData.map(item => parseFloat(item.pctChange));

      this.datacollection = {
        labels: labels,
        datasets: [
          {
            label: 'Variação Percentual (USD/BRL)',
            backgroundColor: '#42A5F5',
            data: values
          }
        ]
      };
    } catch (error) {
      console.error('Erro ao buscar dados da API:', error);
    }
  }
};
</script>

<style scoped>
.chart-container {
  width: 45%;
  margin: 0 auto;
}

@media (max-width: 768px) {
  .chart-container {
    width: 80%;
    margin-bottom: 40px;
  }
}
</style>