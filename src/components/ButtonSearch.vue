<template>
  <button class="btn-search" @click="handleButton">{{ text }}</button>
</template>

<script>
import { defineComponent } from "vue";
export default defineComponent({
  name: "ButtonSearch",
  props: {
    text: String,
  },
  methods: {
    handleButton() {
      this.$emit("customMethod");
    },
  },
});
</script>

<style scoped>
.btn-search {
  width: 100%;
  font-size: 26px;
  padding: 0.25em 1em;
  margin-bottom: 1em;
}
</style>
