<template>
  <TopHeader title="Weather Nation" />
  <div class="container-main">
    <section v-if="cities.length !== 0" class="section-history">
      <div v-for="city in cities" :key="city" class="box-city">
        <button class="btn-city" @click="cityButton">{{ city }}</button>
      </div>
    </section>

    <input
      class="search-field"
      placeholder="Enter A Country..."
      v-model="inputCity"
      type="text"
    />
    <ButtonSearch text="Search" @customMethod="renderWeather" />
    <DisplayWeather :weather="currentWeather" :lon="lon" :lat="lat" />
    <DisplayForcast :weatherList="weatherList" />
  </div>
</template>

<script lang="ts">
// Child Components
import TopHeader from "./components/TopHeader.vue";
import ButtonSearch from "./components/ButtonSearch.vue";
import DisplayWeather from "./components/DisplayWeather.vue";
import DisplayForcast from "./components/DisplayForcast.vue";

import axios, { Axios } from "axios";
import { Weather } from "./model/weather";
import { defineComponent } from "vue";

export default defineComponent({
  name: "App",
  components: { TopHeader, ButtonSearch, DisplayWeather, DisplayForcast },
  data() {
    return {
      apiKey: process.env.VUE_APP_WEATHER_API_KEY,
      unit: "&units=metric",
      currentWeather: {} as Weather,
      lon: 18,
      lat: 59.3,
      cities: [] as string[],
      inputCity: "",
      city: ("" as string) || null,
      weatherList: [] as Weather[],
    };
  },
  // Run after refreshing the webpage
  mounted() {
    this.fetchCityCoord("Stockholm");
    this.displayCities();
  },

  // Run if lat has been updated
  watch: {
    lat() {
      this.fetchWeather();
      this.fetchWeatherForecast();
    },
  },
  methods: {
    //
    cityButton(e: Event) {
      const target = e.target as HTMLElement;
      const city = target.textContent;
      this.fetchCityCoord(city || "{}");
    },

    // Convert to Time only
    convertTime(dt_txt: string): string {
      const date = new Date(dt_txt);
      const time = date.toLocaleTimeString([], { timeStyle: "short" });
      console.log(time);
      return time;
    },

    // Reset search field
    resetInput() {
      this.inputCity = "";
    },

    // Add city name to cities list
    addCity() {
      const city = this.inputCity;
      if (typeof city === "string") {
        if (this.cities.length > 2) {
          this.cities.splice(-1);
        }
        this.cities.unshift(city);
      }
    },
    // Store cities in browser locale memory
    storeCities() {
      localStorage.setItem("cities", JSON.stringify(this.cities));
    },
    // Display cities from locale memory
    displayCities() {
      this.cities = JSON.parse(localStorage.getItem("cities") || "{}");
    },
    // Render weather data
    renderWeather() {
      this.fetchCityCoord(this.inputCity);
      this.addCity();
      this.storeCities();
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
          this.currentWeather.icon =
            "https://openweathermap.org/img/wn/" +
            res.data.weather[0].icon +
            ".png";
        })
        .catch((err) => console.log(err));
    },
    // Fetch forcasting
    fetchWeatherForecast() {
      const url = `https://api.openweathermap.org/data/2.5/forecast?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}${this.unit}`;
      this.weatherList = [];

      axios
        .get(url)
        .then((res) => {
          for (var i = 0; i < 3; i++) {
            let weather = {} as Weather;
            weather.temp = res.data.list[i].main.temp;
            weather.time = this.convertTime(res.data.list[i].dt_txt);
            weather.icon =
              "https://openweathermap.org/img/wn/" +
              res.data.list[i].weather[0].icon +
              ".png";
            this.weatherList.push(weather);
          }
        })
        .catch((err) => console.error(err));
    },
  },
});
</script>

<style scoped>
body {
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
  padding: 0em 1em;
  border-radius: 7px;
  margin-bottom: 1.5em;
}

.btn-city {
  padding: 0.5em 1em;
}

.search-field {
  width: 100%;
  font-size: 24px;
  padding: 0.25em 0;
  text-align: center;
}
</style>
