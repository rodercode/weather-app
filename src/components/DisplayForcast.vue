<template>
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
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { type PropType } from "vue";
import { Weather } from "../model/weather";

export default defineComponent({
  name: "DisplayForcast",
  props: {
    weatherList: {
      type: Array as PropType<Weather[]>,
    },
    lon: Number,
    lat: Number,
  },
});
</script>

<style scoped>
.subtitle-hourly-forecast {
  text-align: center;
  margin-bottom: 0.25em;
}

.container-hourly-forecast {
  padding: 2em;
  border: 1px solid black;
  margin-bottom: 1.5em;
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
