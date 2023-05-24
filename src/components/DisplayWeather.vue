<template>
  <!-- Display Weather -->
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
  <!-- End of Display Weather -->
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { type PropType } from "vue";
import { Weather } from "../model/weather";

// export interface Weather {
//   temp: number;
//   description: string;
//   humidity: number;
//   pressure: number;
//   wind: number;
//   speed: number;
// }

export default defineComponent({
  name: "DisplayWeather",
  props: {
    weather: {
      type: Object as PropType<Weather>,
      required: true,
    },
  },

  components: {},
});
</script>

<style scoped>
header {
  background-color: rgb(185, 176, 176);
  text-align: center;
  padding: 1.5em;
  margin-bottom: 1em;
}
</style>
