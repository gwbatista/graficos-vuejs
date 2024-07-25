<template>
  <div id="app">
    <div class="charts-container">
      <LineChart />
      <ColumnChart />
    </div>
    <PricesTable />
  </div>
</template>

<script>
import ColumnChart from './components/ColumnChart.vue';
import LineChart from './components/LineChart.vue';
import PricesTable from './components/PricesTable.vue';

export default {
  name: 'App',
  components: {
    LineChart,
    ColumnChart,
    PricesTable
  }
};
</script>

<style>
#app {
  text-align: center;
}

.charts-container {
  display: flex;
  justify-content: space-around;
  margin-bottom: 60px;
}

@media (max-width: 768px) {
  .charts-container {
    flex-direction: column;
    align-items: center;
  }

  .chart-container {
    width: 80%;
  }
}
</style>