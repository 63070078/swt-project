<template>
  <div id="app">
    <nav class="navbar" role="navigation" aria-label="main navigation">
  <div class="navbar-brand">
    <a class="navbar-item" href="https://bulma.io">
      <img src="https://bulma.io/images/bulma-logo.png" width="112" height="28">
    </a>

    <a role="button" class="navbar-burger" aria-label="menu" aria-expanded="false" data-target="navbarBasicExample">
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
    </a>
  </div>

  <div id="navbarBasicExample" class="navbar-menu">
    <div class="navbar-start">
      <a class="navbar-item">
        Home
      </a>

      <a class="navbar-item">
        Documentation
      </a>

      <div class="navbar-item has-dropdown is-hoverable">
        <a class="navbar-link">
          More
        </a>

        <div class="navbar-dropdown">
          <a class="navbar-item">
            About
          </a>
          <a class="navbar-item">
            Jobs
          </a>
          <a class="navbar-item">
            Contact
          </a>
          <hr class="navbar-divider">
          <a class="navbar-item">
            Report an issue
          </a>
        </div>
      </div>
    </div>

    <div class="navbar-end">
      <div class="navbar-item">
        <div class="buttons">
          <a class="button is-primary">
            <strong>Sign up</strong>
          </a>
          <a class="button is-light">
            Log in
          </a>
        </div>
      </div>
    </div>
  </div>
</nav>
    <p>
      
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