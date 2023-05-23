<template>
  <header>
    <h1 class="headline-title">{{ title }}</h1>
  </header>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "TopHeader",
  props: {
    title: String,
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
