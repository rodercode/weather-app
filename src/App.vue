<template>
  <TopHeader title="App Logo/title" />
  <h1>{{ lon }}</h1>
  <h1>{{ lat }}</h1>
  <div class="container-main">
    <section class="section-history">
      <div class="box-city">city_1</div>
      <div class="box-city">city_2</div>
      <div class="box-city">city_3</div>
    </section>

    <input class="search-field" placeholder="Search Field" type="text" />
    <div class="container-weather-data">
      <section class="section-temp">
        <span class="temp-icon">Icon</span>
        <span class="temp-data">{{ weather.temp }}°C</span>
      </section>

      <p class="paragraph-weather-info">{{ weather.description }}</p>

      <section class="section-additonal-weather-info">
        <span class="humidity">{{ weather.humidity }}%</span>
        <span class="pressure">{{ weather.pressure }} hPa</span>
        <span class="wind">{{ weather.wind }} m/s</span>
      </section>
    </div>

    <h3 class="subtitle-hourly-forecast">Hourly forecast</h3>
    <div class="container-hourly-forecast">
      <ul class="list-hourly-forecast">
        <li class="item-hourly-forecast">icon</li>
        <li class="item-hourly-forecast">tempature</li>
        <li class="item-hourly-forecast">time</li>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import TopHeader from "./components/TopHeader.vue";
import axios from "axios";
import { Weather } from "./model/weather";

export default defineComponent({
  name: "App",
  components: { TopHeader },
  data() {
    return {
      apiKey: process.env.VUE_APP_WEATHER_API_KEY,
      city: "Stockholm",
      unit: "&units=metric",
      weather: {} as Weather,
    };
  },
  mounted: function () {
    this.fetchCityCoord();
  },
  methods: {
    // Fetch data from Gecoding
    fetchCityCoord() {
      const url = `https://api.openweathermap.org/geo/1.0/direct?q=${
        this.city
      }&limit=${1}&appid=${this.apiKey}`;

      axios
        .get(url)
        .then((res) => {
          const lon = res.data[0].lon;
          const lat = res.data[0].lat;
          this.fetchWeather(lon, lat)
        })
        .catch((err) => console.log(err));
    },
    // Fetch Weather Data
    fetchWeather(lat:number, lon:number) {
      const url = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${this.apiKey}${this.unit}`;

      axios
        .get(url)
        .then((res) => {
          console.log(res.data);
          this.weather.temp = res.data.main.temp;
          this.weather.description = res.data.weather[0].description;
          this.weather.humidity = res.data.main.humidity;
          this.weather.pressure = res.data.main.pressure;
          this.weather.wind = res.data.wind.speed;
        })
        .catch((err) => console.log(err));
    },
  },
});
</script>

<style scoped>
body,
.container-weather-data,
.container-hourly-forecast,
.box-city {
  border: 1px solid black;
}

.box-city,
.search-field {
  margin-bottom: 1em;
}

.container-main {
  padding-left: 2em;
  padding-right: 2em;
}
.section-history {
  display: flex;
  justify-content: center;
  gap: 0.5em;
}

.box-city {
  padding: 0.5em 2.25em 0 2.25em;
  background-color: rgb(185, 176, 176);
  border-radius: 7px;
}

.search-field {
  width: 100%;

  font-size: 24px;
  padding: 0.25em 0;
  text-align: center;
}
.container-weather-data {
  padding: 2em;
  margin-bottom: 1.5em;
}
.section-temp {
  display: flex;
  justify-content: space-between;

  margin-bottom: 3em;
}
.temp-icon,
.temp-data {
  font-size: 24px;
}
.paragraph-weather-info {
  text-align: center;
  margin-bottom: 2em;
  font-size: 19px;
}

.section-additonal-weather-info {
  display: flex;
  gap: 1em;
}

.subtitle-hourly-forecast {
  text-align: center;
  margin-bottom: 0.25em;
}

.container-hourly-forecast {
  padding: 2em;
}
.list-hourly-forecast {
  display: flex;
  justify-content: center;
  gap: 30px;
}

.item-hourly-forecast {
  font-size: 20px;
}
</style>
