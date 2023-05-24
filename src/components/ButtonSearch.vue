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

<style scoped>
.btn-search {
  width: 100%;
  font-size: 26px;
  padding: 0.25em 1em;
  margin-bottom: 1em;
}
</style>
