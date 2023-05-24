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
    <DisplayWeather :weather="weather" :lon="lon" :lat="lat" />

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
// Child Components
import TopHeader from "./components/TopHeader.vue";
import ButtonSearch from "./components/ButtonSearch.vue";
import DisplayWeather from "./components/DisplayWeather.vue";

import axios from "axios";
import { Weather } from "./model/weather";
import { defineComponent } from "vue";

export default defineComponent({
  name: "App",
  components: { TopHeader, ButtonSearch, DisplayWeather },
  data() {
    return {
      apiKey: process.env.VUE_APP_WEATHER_API_KEY,
      unit: "&units=metric",
      weather: {} as Weather,
      lon: 0,
      lat: 0,
      inputCity: "",
    };
  },
  mounted() {
    this.fetchCityCoord("Stockholm");
  },

  watch: {
    lat() {
      this.fetchWeather();
    },
  },
  methods: {
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
          this.weather.city = res.data.name;
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
