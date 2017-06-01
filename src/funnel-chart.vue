<template>
  <div class="funnel-chart">
    <header>
      <h3>漏斗图表</h3>
      <p>用<code>table</code>元素布局，实现漏斗图表和列表结合，实现数据展示</p>
    </header>
    <table style="width: 100%" cellpadding="0" cellspacing="0">
      <thead>
        <tr>
          <td style="min-width: 160px; max-width: 160px; width: 160px">转化率</td>
          <td style="text-align: center;">{{chartTitle}}</td>
          <td style="min-width: 80px; max-width: 80px; width: 80px">值</td>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>
            <div>
              <template v-for="(val, $index) in leftList" $key="$index">
                <div class="cell">{{val.text + ' ' + val.value}}</div>
              </template>
            </div>
          </td>
          <td>
            <div id="funnel-chart" :style="chartStyle"></div>
          </td>
          <td>
            <div>
              <template v-for="(val, $index) in funnelData" $key="$index">
                <div class="cell">{{val.value}}</div>
              </template>
            </div>
          </td>
        </tr>
        <tr>
          <td colspan="3" style="text-align: center;">{{summaryText}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
  import echarts from 'echarts'
  export default {
    name: 'funnel-chart',
    components: {},
    data () {
      return {
        chartTitle: '漏斗标题',  // 漏斗标题
        leftList: [{  // 左转换率数据
          text: '转化率1',
          value: '40%'
        },{
          text: '转化率2',
          value: '30%'
        },{
          text: '转化率3',
          value: '20%'
        },{
          text: '转化率4',
          value: '10%'
        }],
        funnelData: [{
          name: '测试数据1',
          value: 100
        }, {
          name: '测试数据2',
          value: 80
        }, {
          name: '测试数据3',
          value: 60
        }, {
          name: '测试数据4',
          value: 40
        }, {
          name: '测试数据5',
          value: 20
        }],
        summaryText: '总计数据：123456',
        chartStyle: 'height: 140px'  // 图表默认高度
      }
    },
    methods: {
      initChart (data) {  // 初始化表格
        let options = {
          title: {
            show: true,
            name: 'test'
          },
          tooltip: {
            trigger: 'item'
          },
          grid: {
            top: 0,
              left: 0,
              bottom: 0,
              right: 0
          },
          series: [{
            color: ['#00a0e9'],
            name: '',
            type: 'funnel',
            gap: 3,
            top: 0,
            left: 0,
            bottom: 0,
            label: {
              normal: {
                textStyle: {
                  color: '#333'
                }
              }
            },
            labelLine: {
              normal: {
                lineStyle: {
                  color: '#333'
                }
              }
            },
            itemStyle: {
              normal: {}
            },
            data: data
            }
          ]
        }
        let mainChart = echarts.init(document.getElementById('funnel-chart'))
        mainChart.setOption(options)
      }
    },
    mounted () {
      this.chartStyle = `width: inherit; height: ${this.funnelData.length * 35}px`
      setTimeout(() => {
        this.initChart(this.funnelData)
      }, 0)
    }
  }
</script>
<style scoped>
  table, tr, td {
    border: solid 1px #ccc;
  }
  header {
    padding: 10px 0;
    text-align: left;
  }
  .funnel-chart {
    height: 100%;
  }
  #funnel-chart {
    font-size: 16px;
    font-weight: 600;
  }
  .cell {
    height: 35px;
    line-height: 35px;
    border-bottom: solid 1px #ccc;
  }
</style>
