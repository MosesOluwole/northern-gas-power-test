<script>
  import { Line } from 'vue-chartjs'
  export default {
    name: 'LineChart',
    extends: Line,
    props: {
      label: {
        type: String,
      },
      chartData: {
        type: Array,
      },

      options: {
        type: Object,
      },
    },
    mounted () {
      // const dates = this.chartData.map(temp => temp.x)
      const dates = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sept', 'Oct', 'Nov', 'Dec']
      const totals = this.chartData.map(temp => temp)
      this.renderChart(
        {
          labels: dates,
          datasets: [
            {
              label: this.label,
              data: totals,
            },
          ],
        },
        this.Options,
      )
    },
  }
</script>
<style scoped>
</style>
