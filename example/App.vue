<template>
  <div class="app">
    <header>
      <h1>charts yo yo 业务应用图表示例</h1>
    </header>
    <funnel-chart />
    <compare-charts />
  </div>
</template>
<script>
  import FunnelChart from '../src/funnel-chart'
  import CompareCharts from '../src/compare-charts'
  export default {
    name: 'app',
    components: {
      FunnelChart,
      CompareCharts
    }
  }
</script>
<style>
  body {
    margin: 0;
    border-radius: 0 !important;
    background: #F9FAFC;
    font-family: 微软雅黑;
    font-size: 12px;
  }
  header {
    text-align: center;
    padding: 20px 0px;
  }
  .app {
    width: 60%;
    margin: auto;
  }
</style>
