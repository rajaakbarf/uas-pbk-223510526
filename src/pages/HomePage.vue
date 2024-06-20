<template>
  <q-page class="flex flex-center">
    <div class="container">
      <div class="background-image">
        <q-card class="weather-card">
          <q-card-section>
            <div class="header">
              <h2 class="text-center">Widget Weather Realtime</h2>
              <q-input v-model="city" placeholder="Enter the Location" class="city-input" />
              <q-btn class="search-btn" label="Search" @click="fetchWeather" />
            </div>
          </q-card-section>
          <q-card-section v-if="weather" class="main-weather">
            <div class="weather-now">
              <div class="weather-icon">
                <img :src="`http://openweathermap.org/img/wn/${weather.weather[0].icon}@2x.png`" alt="Weather Icon" />
              </div>
              <div class="weather-info">
                <p class="current-time">Current Time<br>{{ currentTime }}</p>
                <p class="temperature">{{ weather.main.temp }}°C</p>
                <p class="location">{{ weather.name }}, {{ weather.sys.country }}</p>
                <p class="description">Description : {{ weather.weather[0].description }}</p>
              </div>
            </div>
          </q-card-section>
          <q-card-section v-if="error">
            <p class="text-red-5">{{ error }}</p>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script>
import axios from 'axios';

export default {
  name: 'WeatherPage',
  data() {
    return {
      city: '',
      weather: null,
      error: '',
      currentTime: '',
    };
  },
  methods: {
    async fetchWeather() {
      const apiKey = '94bf6d59f86ae95e8071e78240546056';
      const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${apiKey}`;
      try {
        const response = await axios.get(url);
        this.weather = response.data;
        this.error = ''; // Reset error message if successful
        this.updateCurrentTime();
      } catch (error) {
        this.error = 'City not found'; // Set error message
        console.error(error);
      }
    },
    updateCurrentTime() {
      const now = new Date();
      const hours = now.getHours().toString().padStart(2, '0');
      const minutes = now.getMinutes().toString().padStart(2, '0');
      this.currentTime = `${hours}:${minutes}`;
    },
  },
  mounted() {
    this.updateCurrentTime();
    setInterval(this.updateCurrentTime, 60000); // Update time every minute
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap');

.q-page {
  background-image: url('https://sm.mashable.com/mashable_in/image/default/uploads252fcard252fimage252f1544055252f15a40f44-eee9-4926-af_rp4f.jpg');
  background-size: cover;
  background-position: center;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.container {
  width: 100%;
  max-width: 800px;
  margin: 0 auto;
  padding: 0;
  font-family: 'Roboto', sans-serif;
  position: relative;
  z-index: 2;
}

.weather-card {
  width: 100%;
  padding: 10px;
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  background-color: rgba(249, 249, 249, 0.7);
  color: #ffffff;
}

.header {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.city-input {
  margin-top: 10px;
  width: 100%;
}

.search-btn {
  margin-top: 10px;
  background-color: #00aaff;
  color: #ffffff;
  width: 100%;
}

.main-weather {
  display: flex;
  flex-direction: column;
  margin-top: 20px;
}

.weather-now {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: black;
  border-radius: 10px;
  opacity: 80%;
  padding: 20px;
}

.weather-icon img {
  width: 65px;
  height: 65px;
}

.weather-info {
  text-align: center;
  margin-top: 10px;
}

.current-time {
  font-size: 1.2rem;
  margin-bottom: 10px;
}

.temperature {
  font-size: 2.5rem;
}

.location {
  font-size: 1.2rem;
}

.description {
  margin-bottom: 20px;
}

.text-red-5 {
  color: #f44336;
  text-align: center;
}

.text-center {
  text-align: center;
}
</style>
