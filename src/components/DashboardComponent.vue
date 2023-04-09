<template>
  <div class="dashboard">
    <div class="dashboard-box">
      <h3 class="dashboard-title">PM2.5</h3>
      <div class="dashboard-value">{{ pm25 }} µg/m³</div>
    </div>
    <div class="dashboard-box">
      <h3 class="dashboard-title">Temperature</h3>
      <div class="dashboard-value">{{ temperature }} °C</div>
    </div>
    <div class="dashboard-box">
      <h3 class="dashboard-title">Humidity</h3>
      <div class="dashboard-value">{{ humidity }} %</div>
    </div>
    <div class="dashboard-box">
      <h3 class="dashboard-title">Wind Speed</h3>
      <div class="dashboard-value">{{ windSpeed }} km/h</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "DashboardComponent",
  props: {
    pm25: {
      type: Number,
      required: true,
    },
    temperature: {
      type: Number,
      required: true,
    },
    humidity: {
      type: Number,
      required: true,
    },
    windSpeed: {
      type: Number,
      required: true,
    },
  },
};
</script>

<style scoped>
.dashboard {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin-bottom: 30px;
}

.dashboard-box {
  background-color: #f7f7f7;
  border-radius: 5px;
  padding: 20px;
  margin-bottom: 20px;
  width: calc(25% - 20px);
}

.dashboard-title {
  font-size: 1.2rem;
  font-weight: bold;
  margin-bottom: 10px;
}

.dashboard-value {
  font-size: 2rem;
  font-weight: bold;
}

@media screen and (max-width: 768px) {
  .dashboard-box {
    width: calc(50% - 10px);
  }
}

@media screen and (max-width: 480px) {
  .dashboard-box {
    width: 100%;
  }
}
</style>
