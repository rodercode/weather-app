<template>
  <TopHeader title="App Logo/title" />
  <div class="container-main">
    <section class="section-history">
      <div class="box-city">city_1</div>
      <div class="box-city">city_2</div>
      <div class="box-city">city_3</div>
    </section>

    <input
      class="search-field"
      placeholder="Enter A Country..."
      v-model="inputCity"
      type="text"
    />
    <ButtonSearch text="Search" @customMethod="renderWeather" />
    <DisplayWeather :weather="currentWeather" :lon="lon" :lat="lat" />

    <h3 class="subtitle-hourly-forecast">Hourly forecast</h3>
    <div class="container-hourly-forecast">
      <ul
        v-for="weather in weatherList"
        :key="weather.temp"
        class="list-hourly-forecast"
      >
        <li class="item-hourly-forecast">icon</li>
        <li class="item-hourly-forecast">{{ weather.temp }}°C</li>
        <li class="item-hourly-forecast">{{ weather.time }}</li>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
// Child Components
import TopHeader from "./components/TopHeader.vue";
import ButtonSearch from "./components/ButtonSearch.vue";
import DisplayWeather from "./components/DisplayWeather.vue";

import axios, { Axios } from "axios";
import { Weather } from "./model/weather";
import { defineComponent } from "vue";

export default defineComponent({
  name: "App",
  components: { TopHeader, ButtonSearch, DisplayWeather },
  data() {
    return {
      apiKey: process.env.VUE_APP_WEATHER_API_KEY,
      unit: "&units=metric",
      currentWeather: {} as Weather,
      lon: 18,
      lat: 59.3,
      inputCity: "",
      weatherList: [] as Weather[],
    };
  },
  mounted() {
    this.fetchCityCoord("Stockholm");
    this.fetchWeatherForecast();
  },

  watch: {
    lat() {
      this.fetchWeather();
    },
  },
  methods: {
    convertTime(dt_txt: string): string {
      const date = new Date(dt_txt);
      const time = date.toLocaleTimeString([], { timeStyle: "short" });
      console.log(time);
      return time;
      // const dateStr = "2023-05-25 16:00:00";
      // const timestamp = new Date(dateStr).getTime() / 1000;

      // const date = new Date(timestamp);
      // const time =
      //   ("0" + date.getHours()).slice(-2) +
      //   ":" +
      //   ("0" + date.getMinutes()).slice(-2);
      // console.log(time);
    },
    resetInput() {
      this.inputCity = "";
    },
    renderWeather() {
      this.fetchCityCoord(this.inputCity);
      this.resetInput();
    },
    // Fetch data from Gecoding
    fetchCityCoord(city: string) {
      const url = `https://api.openweathermap.org/geo/1.0/direct?q=${city}&limit=${1}&appid=${
        this.apiKey
      }`;

      axios
        .get(url)
        .then((res) => {
          this.lon = res.data[0].lon;
          this.lat = res.data[0].lat;
        })
        .catch((err) => console.log(err));
    },
    // Fetch Weather Data
    fetchWeather() {
      const url = `https://api.openweathermap.org/data/2.5/weather?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}${this.unit}`;
      axios
        .get(url)
        .then((res) => {
          this.currentWeather.city = res.data.name;
          this.currentWeather.temp = res.data.main.temp;
          this.currentWeather.description = res.data.weather[0].description;
          this.currentWeather.humidity = res.data.main.humidity;
          this.currentWeather.pressure = res.data.main.pressure;
          this.currentWeather.wind = res.data.wind.speed;
        })
        .catch((err) => console.log(err));
    },
    fetchWeatherForecast() {
      const url = `https://api.openweathermap.org/data/2.5/forecast?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}${this.unit}`;

      axios
        .get(url)
        .then((res) => {
          for (var i = 0; i < 3; i++) {
            let weather = {} as Weather;
            weather.temp = res.data.list[i].main.temp;
            weather.time = this.convertTime(res.data.list[i].dt_txt);
            this.weatherList.push(weather);
          }
        })
        .catch((err) => console.error(err));
    },
  },
});
</script>

<style scoped>
body,
.container-hourly-forecast,
.box-city {
  border: 1px solid black;
  margin-bottom: 1.5em;
}

.search-field {
  margin-bottom: 0.5em;
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
