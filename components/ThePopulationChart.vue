<template>
  <highcharts :options="chartOptions"></highcharts>
</template>

<script>
import { mapState } from 'vuex'
import { Chart } from 'highcharts-vue'
import Highcharts from 'highcharts'

export default {
  components: {
    highcharts: Chart,
  },
  computed: {
    ...mapState(['prefectures', 'populations', 'selectedPrefs']),

    chartOptions() {
      return {
        chart: {
          spacingTop: 65,
          spacingLeft: -35,
        },
        title: {
          text: '',
        },
        tooltip: {
          formatter() {
            return `<b>${this.point.series.name}</b><br>${this.x}年 ${this.y}人`
          },
        },
        xAxis: {
          title: {
            text: '年度',
            align: 'high',
            x: 32,
            y: -18,
          },
        },
        yAxis: {
          title: {
            text: '人口数',
            align: 'high',
            x: 45,
            y: -25,
            rotation: 0,
          },

          labels: {
            x: -5,
            formatter() {
              return Highcharts.numberFormat(this.value, 0, '.', '')
            },
          },
        },
        legend: {
          align: 'right',
          verticalAlign: 'top',
          layout: 'vertical',
        },
        series: this.selectedPrefs.map((v) => {
          return {
            name: this.prefectures[v],
            data: this.populations[v].map((p) => [p.year, p.value]),
          }
        }),
        responsive: {
          rules: [
            {
              condition: {
                maxWidth: 600,
              },
              chartOptions: {
                chart: {
                  spacingLeft: -35,
                },
                xAxis: {
                  title: {
                    align: 'high',
                    x: 0,
                    y: 0,
                  },
                },
                yAxis: {
                  title: {
                    align: 'high',
                    x: 45,
                    y: -25,
                    rotation: 0,
                  },
                  labels: {
                    align: 'left',
                    x: 0,
                    y: -2,
                  },
                },
                legend: {
                  layout: 'horizontal',
                  align: 'center',
                  verticalAlign: 'bottom',
                },
              },
            },
          ],
        },
      }
    },
  },
}
</script>
