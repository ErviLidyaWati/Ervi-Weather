<template>
  <div class="centered-content">
    <div :class="['weather-widget', weatherCondition]">
      <div class="widget-header">
        <h2 class="widget-title">Weather Information</h2>
      </div>
      <q-input
        filled
        v-model="searchQuery"
        label="Enter City Name"
        class="location-input"
      />
      <q-btn @click="searchWeather" class="search-button">Search</q-btn>
      <div v-if="loading" class="loading-message">Loading data...</div>
      <div v-else-if="weatherData" class="weather-result">
        <div class="weather-info">
          <p class="city-name">{{ weatherData.name }}, {{ weatherData.sys.country }}</p>
          <p class="temperature">{{ weatherData.main.temp }}°C</p>
          <img
            :src="getWeatherIconUrl(weatherData.weather[0].icon)"
            :alt="weatherData.weather[0].description"
            class="weather-icon"
          />
          <p class="weather-description">
            {{ weatherData.weather[0].description }}
          </p>
        </div>
        <div class="additional-info">
          <div class="info-item">
            <p>Humidity: {{ weatherData.main.humidity }}%</p>
          </div>
          <div class="info-item">
            <p>Wind Speed: {{ weatherData.wind.speed }} m/s</p>
          </div>
          <div class="info-item">
            <p>Pressure: {{ weatherData.main.pressure }} hPa</p>
          </div>
          <div class="info-item">
            <p>Visibility: {{ (weatherData.visibility / 1000).toFixed(1) }} km</p>
          </div>
        </div>
        <div class="date-time">
          <p>{{ localDate }}</p>
          <p>{{ localTime }}</p>
        </div>
      </div>
      <div v-else-if="error" class="error-message">{{ error }}</div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted } from 'vue';
import axios from 'axios';
import moment from 'moment-timezone';
import { QInput, QBtn } from 'quasar';

export default {
  components: { QInput, QBtn },
  setup() {
    const searchQuery = ref('');
    const weatherData = ref(null);
    const loading = ref(false);
    const error = ref(null);
    const weatherCondition = ref('');
    const localDate = ref('');
    const localTime = ref('');
    const intervalId = ref(null);

    const searchWeather = async () => {
      loading.value = true;
      error.value = null;
      weatherCondition.value = '';
      localDate.value = '';
      localTime.value = '';

      if (intervalId.value) {
        clearInterval(intervalId.value);
      }

      try {
        const response = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?q=${searchQuery.value}&appid=bcbc6cc1860d02bd1f4306314c08d7e0&units=metric`
        );
        if (response.data.cod !== 200) {
          throw new Error('City not found');
        }
        weatherData.value = response.data;
        updateWeatherCondition(response.data.weather[0].main);
        updateLocalTime(response.data.timezone);
        startClock(response.data.timezone);
      } catch (error) {
        console.error(error);
        error.value = 'Failed to fetch weather data or city not found';
      } finally {
        loading.value = false;
      }
    };

    const updateLocalTime = (timezoneOffset) => {
      const utcTime = moment.utc();
      const localMoment = utcTime.utcOffset(timezoneOffset / 60);
      localDate.value = localMoment.format('dddd, MMMM Do YYYY');
      localTime.value = localMoment.format('h:mm:ss a');
    };

    const startClock = (timezoneOffset) => {
      intervalId.value = setInterval(() => {
        const utcTime = moment.utc();
        const localMoment = utcTime.utcOffset(timezoneOffset / 60);
        localDate.value = localMoment.format('dddd, MMMM Do YYYY');
        localTime.value = localMoment.format('h:mm:ss a');
      }, 1000);
    };

    const updateWeatherCondition = (mainWeather) => {
      switch (mainWeather.toLowerCase()) {
        case 'clear':
          weatherCondition.value = 'clear';
          break;
        case 'clouds':
          weatherCondition.value = 'cloudy';
          break;
        case 'rain':
        case 'drizzle':
          weatherCondition.value = 'rainy';
          break;
        case 'snow':
          weatherCondition.value = 'snowy';
          break;
        case 'thunderstorm':
          weatherCondition.value = 'thunderstorm';
          break;
        default:
          weatherCondition.value = 'default';
      }
    };

    const getWeatherIconUrl = (iconCode) =>
      `https://openweathermap.org/img/wn/${iconCode}.png`;

    onUnmounted(() => {
      if (intervalId.value) {
        clearInterval(intervalId.value);
      }
    });

    return {
      searchQuery,
      weatherData,
      loading,
      error,
      searchWeather,
      getWeatherIconUrl,
      weatherCondition,
      localDate,
      localTime,
    };
  },
};
</script>

<style scoped>
.centered-content {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: calc(100vh - 60px);
  margin-top: 90px;
}

.weather-widget {
  width: 600px;
  padding: 20px;
  border-radius: 10px;
  text-align: center;
  backdrop-filter: blur(5px);
  transition: background-image 0.5s ease-in-out;
  background-size: cover;
  background-position: center;
  color: #ffffff;
}

/* Specific background images for each weather condition */
.weather-widget.clear {
  background-image: url('https://i.pinimg.com/474x/24/e9/f7/24e9f7f69d1032757bf8a1709cdde17d.jpg');
}

.weather-widget.cloudy {
  background-image: url('https://i.pinimg.com/474x/df/b1/db/dfb1db002e32f16e90f1869d18e9a98e.jpg');
}

.weather-widget.rainy {
  background-image: url('https://i.pinimg.com/474x/63/21/e1/6321e181eba7f7e9cf5fcfa685701580.jpg');
}

.weather-widget.snowy {
  background-image: url('https://i.pinimg.com/474x/19/bd/82/19bd825a713d91dce0000f6042e24560.jpg');
}

.weather-widget.thunderstorm {
  background-image: url('https://i.pinimg.com/474x/e9/b5/79/e9b5790c957d2f9aa03c2d2f6d2db533.jpg');
}

.weather-widget.default {
  background-image: url('https://i.pinimg.com/474x/4b/fb/04/4bfb04666f077bb2cd6056495f10d01f.jpg');
}

.widget-header {
  text-align: center;
  margin-bottom: 15px;
}

.widget-title {
  font-size: 1.8rem;
  color: #ffffff;
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande',
    'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
}

.location-input {
  width: calc(100% - 10px);
  padding: 12px;
  border: 1px solid #0a0a0a;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin: center;
  background-color: rgba(240, 240, 240, 0.8);
}

.search-button {
  width: 110px;
  padding: 12px 25px;
  background-color: #4caf50;
  color: #fff;
  border: none;
  border-radius: 7px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  cursor: pointer;
  transition: background-color 0.3s ease;
  margin-top: 8px;
}

.search-button:hover {
  background-color: #45a049;
}

.loading-message {
  font-style: italic;
  color: #ffffff;
  margin-top: 10px;
}

.weather-result {
  margin-top: 20px;
  padding: 15px;
  border-radius: 5px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  transition: box-shadow 0.3s ease, transform 0.3s ease;
  background-color: rgba(0, 0, 0, 0.7);
}

.weather-result:hover {
  box-shadow: 0px 0px 15px rgba(0, 0, 0, 0.2);
  transform: translateY(-5px);
}

.weather-info {
  margin-bottom: 10px;
  text-align: center;
}

.city-name {
  font-weight: bold;
  color: #ffffff;
  font-size: 1.5rem;
}

.temperature {
  font-size: 3rem;
  color: #ffffff;
}

.weather-icon {
  width: 100px;
  height: 100px;
  margin: 0 auto;
  display: block;
}

.weather-description {
  font-style: italic;
  color: #ffffff;
}

.additional-info {
  margin-top: 20px;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}

.info-item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.info-icon {
  width: 30px;
  height: 30px;
  margin-right: 10px;
}

.info-item p {
  color: #ffffff;
}

.date-time {
  margin-top: 10px;
  font-size: 1.2rem;
  color: #ffffff;
}
</style>
