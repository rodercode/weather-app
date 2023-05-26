<template>
  <div v-if="lon !== 0 || lat !== 0" class="container-weather-data">
    <h3 class="city-title">{{ weather.city }}</h3>
    <section class="section-temp">
      <img :src="weather.icon" alt="" />
      <span class="temp-data">{{ weather.temp }}°C</span>
    </section>

    <p class="paragraph-weather-info">{{ weather.description }}</p>

    <section class="section-additonal-weather-info">
      <span class="humidity">humidity {{ weather.humidity }}%</span>
      <span class="pressure">{{ weather.pressure }} hPa</span>
      <span class="wind">{{ weather.wind }} m/s</span>
    </section>
  </div>
  <div v-else>
    <h3>Loading Weather Data...</h3>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { type PropType } from "vue";
import { Weather } from "../model/weather";

export default defineComponent({
  name: "DisplayWeather",
  props: {
    weather: {
      type: Object as PropType<Weather>,
      required: true,
    },
    lon: Number,
    lat: Number,
  },
});
</script>

<style scoped>
.container-weather-data {
  padding: 1em 2em 2em 2em;
  border: 1px solid black;
  margin-bottom: 1.5em;
}

.city-title {
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
</style>
