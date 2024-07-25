<template>
  <div class="table-container">
    <h1>Tabela de Cotações</h1>
    <table v-if="prices.length">
      <thead>
        <tr>
          <th>Code</th>
          <th>Name</th>
          <th>Max</th>
          <th>Min</th>
          <th>pctChange</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="price in prices" :key="price.code">
          <td>{{ price.code }}</td>
          <td>{{ price.name }}</td>
          <td>{{ price.max }}</td>
          <td>{{ price.min }}</td>
          <td>{{ price.pctChange }}</td>
        </tr>
      </tbody>
    </table>
    <p v-else>Carregando dados...</p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      prices: []
    };
  },
  async mounted() {
    try {
      const response = await axios.get('https://economia.awesomeapi.com.br/json/all');
      const data = response.data;

      this.prices = Object.keys(data).map(key => {
        const item = data[key];
        return {
          code: item.code,
          name: item.name,
          max: parseFloat(item.high),
          min: parseFloat(item.low),
          pctChange: parseFloat(item.pctChange)
        };
      });
    } catch (error) {
      console.error('Erro ao buscar dados da API:', error);
    }
  }
};
</script>

<style scoped>
.table-container {
  width: 80%;
  margin: 0 auto;
  margin-bottom: 80px;
}
table {
  width: 100%;
  border-collapse: collapse;
}
th, td {
  border: 1px solid #ddd;
  padding: 8px;
}
th {
  background-color: #f2f2f2;
}
</style>
