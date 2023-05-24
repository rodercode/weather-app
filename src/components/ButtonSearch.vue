<template>
  <button class="btn-search">{{ text }}</button>
</template>

<script>
import { defineComponent } from "vue";
export default defineComponent({
  name: "ButtonSearch",
  props: {
    text: String,
  },
  components: {},
});
</script>

<style></style>
