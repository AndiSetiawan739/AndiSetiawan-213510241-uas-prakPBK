<template>
  <div class="weather-app">
    <div class="weather-container">
      <h1 class="app-title">Weather App</h1>
      <div class="input-container">
        <input v-model="location" type="text" placeholder="Enter location" @keyup.enter="fetchWeatherData" />
        <button @click="fetchWeatherData">Get Weather</button>
      </div>
      <div v-if="loading" class="loading-spinner">
        <div class="spinner"></div>
      </div>
      <div class="weather-data" v-if="weatherData && !loading">
        <div class="weather-icon">
          <img :src="getWeatherIconURL(weatherData.weather[0].icon)" :alt="weatherData.weather[0].main" />
        </div>
        <div class="weather-details">
          <h2 class="weather-main">{{ weatherData.weather[0].main }}</h2>
          <p class="weather-description">{{ weatherData.weather[0].description }}</p>
        </div>
        <div class="temperature">
          <h2 class="temperature-value">{{ convertKelvinToCelcius(weatherData.main.temp) }}°C</h2>
        </div>
      </div>
      <div v-if="error && !loading" class="error-message">
        {{ error }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      location: '',
      weatherData: null,
      error: null,
      loading: false
    };
  },
  methods: {
    async fetchWeatherData() {
  try {
    if (this.location === '') {
      throw new Error('Location cannot be empty');
    }

    this.loading = true; // Set loading to true when fetching data
    const apiKey = 'b7bfca7b27a3485144fea086c50d09dc';
    const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${this.location}&appid=${apiKey}`;

    const response = await fetch(apiUrl);
    const data = await response.json();

    if (data.cod === '404') {
      throw new Error('Location not found');
    }

    this.weatherData = data;
    this.error = null;
  } catch (error) {
    console.error('Error fetching weather data:', error);
    this.weatherData = null;
    this.error = error.message;
  } finally {
    this.loading = false; // Set loading to false after fetching data
  }
},

    getWeatherIconURL(iconCode) {
      return `https://openweathermap.org/img/w/${iconCode}.png`;
    },
    convertKelvinToCelcius(temp) {
      return Math.round(temp - 273.15);
    }
  }
};
</script>

<style>
.weather-app {
  display: flex;
  justify-content: center;
  height:auto;
  
  font-family: 'Arial', sans-serif;
}

.weather-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #111;
  padding: 30px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
  max-width: 400px;
  width: 100%;
}

.app-title {
  font-size: 28px;
  font-weight: bold;
  margin-bottom: 20px;
  color: #2196f3;
}

.input-container {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

input {
  width: 250px;
  padding: 8px;
  font-size: 16px;
  border-radius: 4px 0 0 4px;
  border: none;
  background-color: #222;
  color: #fff;
}

button {
  padding: 8px 16px;
  font-size: 16px;
  border-radius: 0 4px 4px 0;
  border: none;
  background-color: #2196f3;
  color: #fff;
  cursor: pointer;
}

.weather-data {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 20px;
}

.weather-icon img {
  width: 80px;
  height: 80px;
}

.weather-details {
  margin-top: 10px;
  text-align: center;
}

.weather-main {
  font-size: 24px;
  font-weight: bold;
  color: #fff;
}

.weather-description {
  font-size: 16px;
  color: #ccc;
}

.temperature {
  margin-top: 20px;
  text-align: center;
}

.temperature-value {
  font-size: 48px;
  font-weight: bold;
  color: #fff;
}

.error-message {
  margin-top: 20px;
  color: #ff5252;
  text-align: center;
}

.loading-spinner {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}

.spinner {
  border: 4px solid rgba(255, 255, 255, 0.3);
  border-top: 4px solid #fff;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@media (max-width: 600px) {
  .weather-container {
    padding: 10px;
  }

  .app-title {
    font-size: 24px;
  }

  input {
    width: 150px;
    font-size: 14px;
  }

  button {
    font-size: 14px;
  }

  .weather-icon img {
    width: 60px;
    height: 60px;
  }

  .weather-main {
    font-size: 20px;
  }

  .weather-description {
    font-size: 14px;
  }

  .temperature-value {
    font-size: 36px;
  }
}
</style>
