<template>
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
        <span class="temp-data">Temparture</span>
      </section>

      <p class="paragraph-weather-info">weather description</p>

      <section class="section-additonal-weather-info">
        <span class="humidity">humidity</span>
        <span class="pressure">pressure</span>
        <span class="wind">wind</span>
        <span class="speed">speed</span>
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

export default defineComponent({
  name: "App",
  components: {},
});
</script>

<style>


</style>
