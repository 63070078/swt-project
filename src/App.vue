<template>
  <div id="app">
    <nav class="navbar" role="navigation" aria-label="main navigation">
      <!-- ... (ไม่มีการแก้ไขในส่วนนี้) -->
    </nav>
    <p>
      PM2.5: {{ pm25 }}
    </p>
    <DashboardComponent
      :pm25="pm25"
      :temperature="temperature"
      :humidity="humidity"
      :windSpeed="windSpeed"
    />

    <ChartPage :chartData="chartData" :chartOptions="chartOptions" />
  </div>
</template>

<script>
import DashboardComponent from '@/components/DashboardComponent.vue'
import ChartPage from '@/components/ChartPage.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    DashboardComponent,
    ChartPage
  },
  data() {
    return {
      pm25: null,
      temperature: null,
      humidity: null,
      windSpeed: null,
      chartData: null,
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
        scales: {
          xAxes: [
            {
              type: 'time',
              time: {
                unit: 'hour',
                displayFormats: {
                  hour: 'MMM D, hA'
                }
              }
            }
          ],
          yAxes: [
            {
              ticks: {
                beginAtZero: true
              }
            }
          ]
        }
      }
    }
  },
  mounted() {
    this.fetchData()
    setInterval(() => {
      this.fetchData()
    }, 60000)
  },
  methods: {
    fetchData() {
      axios.get('https://api.airvisual.com/v2/city?city=Bangkok&state=Bangkok&country=Thailand&key=9552d3af-4bb6-4b64-a2c0-b7c5640b3ba5&history=24')
        .then(res => {
          const { pm25, temperature, humidity, wind_speed } = res.data.data.current.weather
          this.pm25 = pm25
          this.temperature = temperature
          this.humidity = humidity
          this.windSpeed = wind_speed
        })
        .catch(error => {
          console.error(error)
        })

        axios.get('https://api.airvisual.com/v2/city?city=Bangkok&state=Bangkok&country=Thailand&key=9552d3af-4bb6-4b64-a2c0-b7c5640b3ba5&history=24')
          .then(res => {
            const { hourly } = res.data.data
            const labels = hourly.map(item => item.timestamp_local)
            const pm25Data = hourly.map(item => item.pollution.pm25)
            this.chartData = {
              labels,
              datasets: [
                {
                  label: 'PM2.5',
                  backgroundColor: 'rgba(255, 99, 132, 0.2)',
                  borderColor: 'rgba(255, 99, 132, 1)',
                  borderWidth: 1,
                  hoverBackgroundColor: 'rgba(255, 99, 132, 0.4)',
                  hoverBorderColor: 'rgba(255, 99, 132, 1)',
                  data: pm25Data
                }
              ]
            }
          })
          .catch(error => {
            console.error(error)
          })
}}}
</script>
<style>
@import 'bulma/css/bulma.css';

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  /* background-color: black;; */
  /* display: flex;
  flex-direction: column; */
}
</style>