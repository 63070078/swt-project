<template>
  <div class="chart-page">
    <h2>PM2.5 Chart</h2>
    <line-chart :data="chartData" :options="chartOptions" />
  </div>
</template>

<script>
import { Line, mixins } from 'vue-chartjs'
const { reactiveProp } = mixins

export default {
  extends: Line,
  mixins: [reactiveProp],
  props: ['chartData', 'chartOptions']
}
</script>

<style scoped>
.chart-page {
  margin: 30px auto;
  max-width: 800px;
}

h2 {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 20px;
}
</style>
