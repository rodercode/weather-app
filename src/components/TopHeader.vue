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
