<template>
  <component :is="weatherComponent" />
  <div class="container">
    <div class="search-wrap">
      <label
        class="search-label"
        :class="{ 'input-not-empty-search-label': inputNotEmpty }"
        title="Найти город"
      >
        <img src="../public/search.svg" alt="" class="search-icon" />
        <input
          type="text"
          placeholder="Город"
          class="search-input"
          :class="{ 'input-not-empty-search-input': inputNotEmpty }"
          ref="cityInput"
          v-model="cityName"
          @input="handleInput"
        />
      </label>
      <button
        class="search-btn"
        :class="{ 'input-not-empty-search-btn': inputNotEmpty }"
        @click="getWeatherAndDateInfo"
      >
        Поиск
      </button>
    </div>
    <div class="weather-info" v-show="weatherDataReceived">
      <span class="city-name">{{ lastSearchedCity }}</span>

      <div class="add-weather-info">
        <span class="wind-speed">Скорость ветра: {{ windSpeed }} м/с</span>
        <span class="humidity">Влажность: {{ humidity }}%</span>
      </div>
      <span class="temp">{{ temp }}°C</span>
      <div class="date-wrap">
        <span class="date">{{ date }}</span>
        <span class="time">{{ time }}</span>
      </div>
    </div>
  </div>
  <Loader v-show="loaderShow" />
  <Alert
    :message="alertMessage"
    v-show="alertShow"
    @closeAlert="handleAlertClose"
  />
</template>

<script>
import ClearSunriseWeather from "./components/ClearSunriseWeather.vue";
import ClearMiddayWeather from "./components/ClearMiddayWeather.vue";
import ClearSunsetWeather from "./components/ClearSunsetWeather.vue";
import ClearNightWeather from "./components/ClearNightWeather.vue";
import CloudsSunriseWeather from "./components/CloudsSunriseWeather.vue";
import CloudsMiddayWeather from "./components/CloudsMiddayWeather.vue";
import CloudsSunsetWeather from "./components/CloudsSunsetWeather.vue";
import CloudsNightWeather from "./components/CloudsNightWeather.vue";
import RainWeather from "./components/RainWeather.vue";
import RainNightWeather from "./components/RainNightWeather.vue";
import SnowWeather from "./components/SnowWeather.vue";
import SnowNightWeather from "./components/SnowNightWeather.vue";
import FoggyWeather from "./components/FoggyWeather.vue";
import FoggyNightWeather from "./components/FoggyNightWeather.vue";
import Loader from "./components/Loader.vue";
import Alert from "./components/Alert.vue";

export default {
  data() {
    return {
      inputNotEmpty: false,
      loaderShow: false,
      weatherDataReceived: false,
      weather: "",
      timestamp: null,
      sunrise: null,
      sunset: null,
      weatherComponent: "ClearMiddayWeather",
      alertMessage: "",
      alertShow: false,
      cityName: "",
      lastSearchedCity: "",
      temp: null,
      windSpeed: null,
      humidity: null,
      date: "",
      time: "",
    };
  },
  methods: {
    async getWeatherAndDateInfo() {
      this.weatherDataReceived = false;
      this.loaderShow = true;
      this.lastSearchedCity = this.cityName.trim();
      this.cityName = "";
      this.inputNotEmpty = false;

      try {
        const weatherResponse = await fetch(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.lastSearchedCity}&units=metric&appid=b5c952878f87e59251c909263387e508`
        );
        if (weatherResponse.ok) {
          const weatherData = await weatherResponse.json();

          this.weather = weatherData.weather[0].main.toLowerCase();
          this.sunrise = weatherData.sys.sunrise;
          this.sunset = weatherData.sys.sunset;

          const lon = weatherData.coord.lon;
          const lat = weatherData.coord.lat;

          const dateResponse = await fetch(
            `https://api.timezonedb.com/v2.1/get-time-zone?key=Z9YDYZL3HLL0&format=json&by=position&lat=${lat}&lng=${lon}`
          );

          const dateData = await dateResponse.json();

          this.timestamp = dateData.timestamp;
          const dateAndTime = new Date(this.timestamp * 1000);

          const dateFormatter = new Intl.DateTimeFormat("ru-RU", {
            day: "numeric",
            month: "long",
            year: "numeric",
            timeZone: "UTC",
            hour12: false,
          });
          const formattedDate = dateFormatter.format(dateAndTime);

          const timeFormatter = new Intl.DateTimeFormat("ru-RU", {
            hour: "numeric",
            minute: "numeric",
            timeZone: "UTC",
            hour12: false,
          });
          const formattedTime = timeFormatter.format(dateAndTime);
          this.weatherDataReceived = true;

          this.temp = Math.round(weatherData.main.temp);
          this.windSpeed = weatherData.wind.speed;
          this.humidity = weatherData.main.humidity;
          this.date = formattedDate;
          this.time = formattedTime;

          const sunriseTime = new Date(weatherData.sys.sunrise * 1000);
          const sunsetTime = new Date(weatherData.sys.sunset * 1000);

          sunriseTime.setUTCSeconds(
            sunriseTime.getUTCSeconds() + weatherData.timezone
          );
          sunsetTime.setUTCSeconds(
            sunsetTime.getUTCSeconds() + weatherData.timezone
          );

          const formattedSunriseTimestamp = Math.floor(
            sunriseTime.getTime() / 1000
          );
          this.sunrise = formattedSunriseTimestamp;

          const formattedSunsetTimestamp = Math.floor(
            sunsetTime.getTime() / 1000
          );
          this.sunset = formattedSunsetTimestamp;

          const weatherConditions = {
            clear: () => {
              if (
                this.timestamp >= this.sunrise - 3600 &&
                this.timestamp <= this.sunrise + 3600
              ) {
                this.weatherComponent = "ClearSunriseWeather";
              } else if (
                this.timestamp >= this.sunrise + 3600 &&
                this.timestamp <= this.sunset - 3600
              ) {
                this.weatherComponent = "ClearMiddayWeather";
              } else if (
                this.timestamp >= this.sunset - 3600 &&
                this.timestamp <= this.sunset + 3600
              ) {
                this.weatherComponent = "ClearSunsetWeather";
              } else {
                this.weatherComponent = "ClearNightWeather";
              }
            },
            clouds: () => {
              if (
                this.timestamp >= this.sunrise - 3600 &&
                this.timestamp <= this.sunrise + 3600
              ) {
                this.weatherComponent = "CloudsSunriseWeather";
              } else if (
                this.timestamp >= this.sunrise + 3600 &&
                this.timestamp <= this.sunset - 3600
              ) {
                this.weatherComponent = "CloudsMiddayWeather";
              } else if (
                this.timestamp >= this.sunset - 3600 &&
                this.timestamp <= this.sunset + 3600
              ) {
                this.weatherComponent = "CloudsSunsetWeather";
              } else {
                this.weatherComponent = "CloudsNightWeather";
              }
            },
            rain: () => {
              if (
                this.timestamp >= this.sunrise - 3600 &&
                this.timestamp <= this.sunset + 3600
              ) {
                this.weatherComponent = "RainWeather";
              } else if (
                this.timestamp >= this.sunset + 3600 ||
                this.timestamp <= this.sunrise - 3600
              ) {
                this.weatherComponent = "RainNightWeather";
              }
            },
            snow: () => {
              if (
                this.timestamp >= this.sunrise - 3600 &&
                this.timestamp <= this.sunset + 3600
              ) {
                this.weatherComponent = "SnowWeather";
              } else if (
                this.timestamp >= this.sunset + 3600 ||
                this.timestamp <= this.sunrise - 3600
              ) {
                this.weatherComponent = "SnowNightWeather";
              }
            },
            fog: () => {
              if (
                this.timestamp >= this.sunrise - 3600 &&
                this.timestamp <= this.sunset + 3600
              ) {
                this.weatherComponent = "FoggyWeather";
              } else if (
                this.timestamp >= this.sunset + 3600 ||
                this.timestamp <= this.sunrise - 3600
              ) {
                this.weatherComponent = "FoggyNightWeather";
              }
            },
            mist: () => {
              if (
                this.timestamp >= this.sunrise - 3600 &&
                this.timestamp <= this.sunset + 3600
              ) {
                this.weatherComponent = "FoggyWeather";
              } else if (
                this.timestamp >= this.sunset + 3600 ||
                this.timestamp <= this.sunrise - 3600
              ) {
                this.weatherComponent = "FoggyNightWeather";
              }
            },
            smoke: () => {
              if (
                this.timestamp >= this.sunrise - 3600 &&
                this.timestamp <= this.sunset + 3600
              ) {
                this.weatherComponent = "FoggyWeather";
              } else if (
                this.timestamp >= this.sunset + 3600 ||
                this.timestamp <= this.sunrise - 3600
              ) {
                this.weatherComponent = "FoggyNightWeather";
              }
            },
          };
          const condition = weatherConditions[this.weather];
          if (condition) {
            condition();
          }
        } else {
          this.alertMessage = "Город не найден";
          this.alertShow = true;
          this.weatherDataReceived = false;
        }
      } catch (error) {
        this.alertMessage = "Нет подключения к интернету";
        this.alertShow = true;
        this.weatherDataReceived = false;
      } finally {
        this.loaderShow = false;
      }
    },

    handleInput() {
      this.inputNotEmpty = this.$refs.cityInput.value.trim().length > 0;
    },

    handleAlertClose() {
      this.alertShow = false;
    },
  },
  components: {
    ClearSunriseWeather,
    ClearMiddayWeather,
    ClearSunsetWeather,
    ClearNightWeather,
    CloudsSunriseWeather,
    CloudsMiddayWeather,
    CloudsSunsetWeather,
    CloudsNightWeather,
    RainWeather,
    RainNightWeather,
    SnowWeather,
    SnowNightWeather,
    FoggyWeather,
    FoggyNightWeather,
    Loader,
    Alert,
  },
};
</script>

<style scoped>
.container {
  width: 100vw;
  height: 100dvh;
  padding: 0.625rem;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: space-between;
}

.search-wrap {
  width: max-content;
  display: flex;
  align-items: center;
  gap: 0.625rem;
}

.search-label {
  background: #1d1d1d80;
  border-radius: 0.625rem;
  padding: 0.625rem 0 0.625rem 0.625rem;
  display: flex;
  gap: 0.625rem;
  align-items: center;
}

.search-icon {
  cursor: pointer;
}

.search-label:focus-within {
  padding: 0.625rem;
}

.input-not-empty-search-label {
  padding: 0.625rem;
}

.search-input {
  color: #e4deff;
  background: #00000050;
  width: 0;
  font-size: 1.25rem;
  outline: none;
  border: none;
  border-radius: 0.3125rem;
  transition: 0.3s;
}

.search-input::placeholder {
  color: #e4deffbd;
}

.search-input:focus {
  width: 9.375rem;
  padding: 0.3125rem;
}

.input-not-empty-search-input {
  width: 9.375rem;
  padding: 0.3125rem;
}

.search-btn {
  color: #e4deff;
  height: 100%;
  background: #1d1d1d80;
  border: none;
  border-radius: 0.625rem;
  padding: 0.3125rem 0.625rem;
  font-size: 1.25rem;
  cursor: pointer;
  visibility: hidden;
  opacity: 0;
  transition: 0.3s;
}

.input-not-empty-search-btn {
  visibility: visible;
  opacity: 1;
}

.weather-info {
  color: #e4deff;
  width: 100%;
  height: max-content;
  background: #1d1d1d80;
  border-radius: 0.625rem;
  padding: 0.625rem;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(2, 1fr);
}

.city-name {
  font-size: 1.875rem;
  text-align: center;
  grid-column: span 3;
}

.temp {
  font-size: 2.625rem;
  text-align: center;
}

.add-weather-info {
  font-size: 1.25rem;
  display: flex;
  flex-direction: column;
  gap: 0.375rem;
  white-space: nowrap;
}

.date-wrap {
  text-align: right;
  display: flex;
  flex-direction: column;
  white-space: nowrap;
}

.date {
  font-size: 1.125rem;
}

.time {
  font-size: 1.5rem;
}

@media (max-width: 860px) {
  .weather-info {
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: repeat(3, 1fr);
    align-items: center;
  }

  .city-name {
    grid-column: span 2;
  }

  .add-weather-info {
    grid-column: 1;
    grid-row: 3;
  }

  .temp {
    text-align: left;
    grid-column: 1;
    grid-row: 2;
  }

  .date-wrap {
    grid-column: 2;
    grid-row: span 2;
  }

  .date {
    font-size: 1.25rem;
  }

  .time {
    font-size: 2.625rem;
  }

  @media (max-width: 568px) {
    .add-weather-info {
      font-size: 1rem;
    }

    .temp {
      font-size: 2.25rem;
    }

    .date-wrap {
      font-size: 1.125rem;
    }

    .date {
      font-size: 1rem;
    }

    .time {
      font-size: 2.25rem;
    }
  }

  @media (max-width: 425px) {
    .search-label {
      padding: 0.375rem 0 0.375rem 0.375rem;
      gap: 0.375rem;
    }

    .search-label:focus-within {
      padding: 0.375rem;
    }

    .input-not-empty-search-label {
      padding: 0.375rem;
    }
    .search-input {
      font-size: 1.125rem;
    }
    .search-input:focus {
      width: 7.5rem;
    }

    .input-not-empty-search-input {
      width: 7.5rem;
    }

    .search-btn {
      padding: 0.3125rem;
      font-size: 1.125rem;
    }
    .weather-info {
      grid-template-columns: 1fr;
      grid-template-rows: repeat(4, max-content);
      row-gap: 0.625rem;
    }

    .city-name {
      grid-column: 1 / 2;
      grid-row: 1 / 2;
    }
    .add-weather-info {
      grid-column: 1 / 2;
      grid-row: 3 / 4;
      text-align: center;
      font-size: 1.25rem;
    }

    .temp {
      grid-column: 1 / 2;
      grid-row: 2 / 3;
      text-align: center;
      font-size: 2.875rem;
      line-height: 2.5rem;
    }

    .date-wrap {
      grid-column: 1 / 2;
      grid-row: 4 / 5;
      text-align: center;
    }

    .date {
      font-size: 1.25rem;
    }

    .time {
      font-size: 2.5rem;
    }
  }
}
</style>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Arial, Helvetica, sans-serif;
}
</style>
