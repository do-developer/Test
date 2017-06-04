<template>
  <div class="compare-charts-panel">
    <header>
      <p class="title">对比图</p>
      <p>关联两个图表做数据对比</p>
    </header>
    <div id="main-chart"></div>
    <div id="compare-chart"></div>
  </div>
</template>
<script>
  import echarts from 'echarts'
  export default {
    name: 'compare-charts',
    data () {
      return {
        mainData: {
          legend: ['柱状图1', '柱状图2', '百分比1'],
          xData: ['a', 'b', 'c', 'd', 'e', 'f', 'g'],
          yData: [{
            name: '柱状图1',
            type: 'bar',
            barMaxWidth: 20,
            data: [23, 34, 14, 54, 12, 11, 34]
          }, {
            name: '柱状图2',
            type: 'bar',
            barMaxWidth: 20,
            data: [12, 34, 23, 45, 25, 37, 15]
          }, {
            name: '百分比1',
            type: 'line',
            yAxisIndex: 1,
            data: [23, 14, 34, 25, 14, 16, 26]
          }]
        },
        compareData: {
          legend: ['柱状图1', '柱状图2', '百分比1'],
          xData: ['a', 'b', 'c', 'd', 'e', 'f', 'g'],
          yData: [{
            name: '柱状图1',
            type: 'bar',
            barMaxWidth: 20,
            data: [14, 34, 25, 15, 26, 27, 31]
          }, {
            name: '柱状图2',
            type: 'bar',
            barMaxWidth: 20,
            data: [26, 31, 25, 13, 36, 24, 21]
          }, {
            name: '百分比1',
            type: 'line',
            yAxisIndex: 1,
            data: [27, 14, 46, 43, 31, 26, 37]
          }]
        }
      }
    },
    methods: {
      initChart () {
        let mainOption = {
          title: {
            text: '主图表',
            textStyle: {
              color: '#333',
              fontSize: 14,
              fontWeight: 'normal'
            }
          },
          color: ['#ed7d31', '#ffc000', '#76a655'],
          grid: {
            top: 105,
            left: 75,
            right: 75,
            bottom: 40
          },
          tooltip: {  // 提示框
            trigger: 'axis',
            axisPointer: {
              type: 'shadow'
            },
            position: function (pos, params, el, elRect, size) {  // 位置
              var obj = {top: 10}
              obj[['left', 'right'][+(pos[0] < size.viewSize[0] / 2)]] = 30
              return obj
            },
            formatter: function (params) {  // 内容格式
              let text = ''
              if (params.length > 0) {
                text += `${params[0].name}<br />`
              }
              for (let i in params) {
                let param = params[i]
                text += `<span style="display:inline-block;margin-right:5px;border-radius:10px;width:9px;height:9px;background-color:${param.color}"></span>`
                if (param.seriesType === 'bar') {
                  text += `${param.seriesName}: ${param.value}<br />`
                } else if (param.seriesType === 'line') {
                  text += `${param.seriesName}: ${param.value}%<br />`
                }
              }
              return text
            }
          },
          legend: {
            show: true,
            top: 30,
            height: 100,
            data: this.mainData.legend
          },
          xAxis: {
            show: true,
            axisTick: {
              alignWithLabel: true
            },
            data: this.mainData.xData
          },
          yAxis: [{
            type: 'value',
            name: '数量'
          }, {
            type: 'value',
            name: '百分比(%)',
            axisLabel: {
              formatter: '{value} %'
            }
          }],
          dataZoom: {
            show: false
          },
          series: this.mainData.yData
        }

        let compareOption = {
          title: {
            text: `对比图`,
            textStyle: {
              color: '#333',
              fontSize: 14,
              fontWeight: 'normal'
            }
          },
          color: ['#ed7d31', '#ffc000', '#76a655'],
          grid: {
            top: 65,
            left: 75,
            right: 75,
            bottom: 40
          },
          tooltip: {
            trigger: 'axis',
            axisPointer: {
              type: 'shadow'
            },
            position: function (pos, params, el, elRect, size) {
              var obj = {top: 10}
              obj[['left', 'right'][+(pos[0] < size.viewSize[0] / 2)]] = 30
              return obj
            },
            formatter: function (params) {
              let text = ''
              if (params.length > 0) {
                text += `${params[0].name}<br />`
              }
              for (let i in params) {
                let param = params[i]
                text += `<span style="display:inline-block;margin-right:5px;border-radius:10px;width:9px;height:9px;background-color:${param.color}"></span>`
                if (param.seriesType === 'bar') {
                  text += `${param.seriesName}: ${param.value}<br />`
                } else if (param.seriesType === 'line') {
                  text += `${param.seriesName}: ${param.value}%<br />`
                }
              }
              return text
            }
          },
          legend: {
            show: false
          },
          xAxis: {
            show: true,
            axisTick: {
              alignWithLabel: true
            },
            data: this.compareData.xData
          },
          yAxis: [{
            type: 'value',
            name: '数量'
          }, {
            type: 'value',
            name: '百分比(%)',
            axisLabel: {
              formatter: '{value} %'
            }
          }],
          dataZoom: {
            show: true
          },
          series: this.compareData.yData
        }

        let mainChart = echarts.init(document.getElementById('main-chart'))
        let compareChart = echarts.init(document.getElementById('compare-chart'))
        mainChart.setOption(mainOption)
        compareChart.setOption(compareOption)
        echarts.connect([mainChart, compareChart])
      }
    },
    mounted () {
      this.initChart()
    }
  }
</script>
<style scoped>
  header {
    padding: 10px 0;
    text-align: left;
  }
  header .title {
    font-size: 16px;
    font-weight: 600;
  }
  .compare-charts-panel {
    height: 500px;
  }
  #main-chart {
    height: 56%;
  }
  #compare-chart {
    height: 44%;
  }
</style>
