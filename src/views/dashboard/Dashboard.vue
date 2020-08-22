<template>
  <v-container
    id="dashboard"
    fluid
    tag="section"
  >
  <div class='wrapper'>
        <drag-it-dude
        @activated="onActivated()"
        @dragging="onDragging()"
        @dropped="onDropped()"
        >
       <base-material-card
          type='Line'
          >
          <template v-slot:heading>
            <div class="display-2 font-weight-light">
              CoinDesk Bitcoin Price Index
            </div>
          </template>
        <line-chart
        class="chart-container"
          v-if="temp.length > 0"
          :chartData="temp"
          :options="chartOptions"
          label="BPI"
          type="Line"
        />
          <template v-slot:actions>
            <v-icon
              class="mr-1"
              small
            >
              mdi-clock-outline
            </v-icon>
            <span class="caption grey--text font-weight-light">updated 3 Seconds ago</span>
          </template>
        </base-material-card>
        </drag-it-dude>
  </div>
  </v-container>
</template>

<script>
  import axios from 'axios'
  import LineChart from './components/core/LineChart'
  import DragItDude from 'vue-drag-it-dude'
  import 'chartjs-plugin-colorschemes'
  export default {
    name: 'DashboardDashboard',
    components: {
      LineChart,
      DragItDude,
    },

    data () {
      return {
        temp: [],
        chartOptions: {
          responsive: true,
          maintainAspectRatio: false,

          plugins: {
            colorschemes: {
              scheme: 'brewer.Paired12',
            },
          },
        },
      }
    },
    async created () {
      const { data } = await axios.get('https://api.coindesk.com/v1/bpi/historical/close.json?start=2019-01-01&end=2019-12-31')
      this.temp = []
      for (const key in data.bpi) {
        var i = data.bpi[key]
        var date = new Date(key)
        if (this.temp[date.getMonth()]) {
          i += this.temp[date.getMonth()]
        }
        this.temp[date.getMonth()] = i
      }
      // console.log(data)
    },
    methods: {
      onActivated () {

      },
      onDragging () {

      },
      onDropped () {
        setTimeout(() => {

        }, 3000)
      },
    },
  }
</script>
<style>
.wrapper {
  width: 80vw;
  height: 80vh;
  position: relative;
}
.drag-it-dude{
width: 20vw;
  height: 50vh;
}
@media only screen and (max-width: 600px) {
 .drag-it-dude {
  width: 90vw;
  height: 53vh;
  }
  .wrapper {
  width: 80vw;
  height: 68vh;
  position: relative;
}
}
/* Medium devices (landscape tablets, 768px and up) */
@media only screen and (min-width: 768px) {
  .drag-it-dude {
    width: 70vw;
  height: 53vh;
  }
}
/* Extra large devices (large laptops and desktops, 1200px and up) */
@media only screen and (min-width: 1200px) {
 .drag-it-dude {
    width: 30vw;
  height: 50vh;
  }
  .wrapper {
  width: 80vw;
  height: 80vh;
  position: relative;
}
}
</style>
