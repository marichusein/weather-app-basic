<template>
  <div id="app">
    <div class="container">
      <h1 class="title">Weather App</h1>
      <div class="search-container">
        <input
          type="text"
          class="search-input"
          placeholder="Enter city name..."
          v-model="city"
          @keyup.enter="getWeather"
        />
        <button class="search-button" @click="getWeather">
          <i class="fas fa-search"></i>
        </button>
      </div>
      <div v-if="loading" class="loading">
        <i class="fas fa-spinner fa-spin"></i>
      </div>
      <div v-else-if="error" class="error">
        {{ error }}
      </div>
      <div v-else-if="weather" class="weather-container">
        <div class="weather-card">
          <div class="weather-header">
            <h2 class="weather-city">{{ weather.name }}</h2>
            <div class="weather-icon">
              <img
                :src="getWeatherIcon(weather.weather[0].icon)"
                :alt="weather.weather[0].description"
              />
            </div>
          </div>
          <div class="weather-details">
            <div class="weather-info">
              <i class="fas fa-thermometer-half"></i>
              <p>{{ weather.main.temp }}°C</p>
            </div>
            <div class="weather-info">
              <i class="fas fa-tint"></i>
              <p>{{ weather.main.humidity }}%</p>
            </div>
            <div class="weather-info">
              <i class="fas fa-wind"></i>
              <p>{{ weather.wind.speed }} m/s</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      city: '',
      weather: null,
      loading: false,
      error: null
    };
  },
  methods: {
    async getWeather() {
      if (this.city === '') return;

      this.loading = true;
      try {
        const apiKey = 'yourapikey';
        const response = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${apiKey}&units=metric`
        );
        this.weather = response.data;
        this.error = null;
      } catch (error) {
        this.weather = null;
        if (error.response && error.response.data && error.response.data.message) {
          this.error = error.response.data.message;
        } else {
          this.error = 'Failed to fetch weather data';
        }
      }
      this.loading = false;
    },
    getWeatherIcon(icon) {
      return `https://openweathermap.org/img/w/${icon}.png`;
    }
  }
};
</script>

<style>
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f4f4f4;
  font-family: Arial, sans-serif;
}

.container {
  max-width: 400px;
  padding: 20px;
  background-color: #ffffff;
  border-radius: 10px;
  box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.1);
}

.title {
  text-align: center;
  margin-bottom: 20px;
  color: #333333;
}

.search-container {
  display: flex;
}

.search-input {
  flex-grow: 1;
  padding: 10px;
  border: 1px solid #cccccc;
  border-radius: 5px 0 0 5px;
}

.search-button {
  padding: 10px;
  background-color: #007bff;
  border: 1px solid #007bff;
  border-radius: 0 5px 5px 0;
  color: #ffffff;
  cursor: pointer;
}

.loading {
  text-align: center;
  margin-top: 20px;
}

.error {
  margin-top: 20px;
  padding: 10px;
  background-color: #ff8080;
  color: #ffffff;
  text-align: center;
}

.weather-container {
  margin-top: 20px;
}

.weather-card {
  padding: 20px;
  background-color: #f0f0f0;
  border-radius: 5px;
}

.weather-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.weather-city {
  margin: 0;
  font-size: 24px;
  color: #333333;
}

.weather-icon {
  width: 50px;
  height: 50px;
}

.weather-details {
  display: flex;
  margin-top: 10px;
}

.weather-info {
  flex-grow: 1;
  display: flex;
  align-items: center;
}

.weather-info i {
  margin-right: 10px;
}

.weather-info p {
  margin: 0;
  color: #333333;
}
</style>
