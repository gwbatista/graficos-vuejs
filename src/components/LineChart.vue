<template>
  <div class= 'chart-container'>
    <h1>Cotação do dólar (USD/BRL) dos últimos 30 dias</h1>
    <line-chart v-if="datacollection && datacollection.labels" :data="datacollection"></line-chart>
    <p v-else>Carregando dados...</p>
  </div>
</template>


<script>
import { Line } from 'vue-chartjs';
import { Chart as ChartJS, CategoryScale, LinearScale, PointElement, LineElement, Title, Tooltip, Legend } from 'chart.js';
import axios from 'axios';

ChartJS.register(CategoryScale, LinearScale, PointElement, LineElement, Title, Tooltip, Legend);

export default {
  components: {
    'line-chart': Line
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

      const labels = data.map(item => new Date(item.timestamp * 1000).toLocaleDateString());
      const values = data.map(item => parseFloat(item.bid));

      this.datacollection = {
        labels: labels,
        datasets: [
          {
            label: 'USD/BRL',
            backgroundColor: '#f87979',
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
