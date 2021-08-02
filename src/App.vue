<template>
  <the-header title="Agile Engine Code Test"></the-header>
  <photos></photos>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import TheHeader from "./components/layout/TheHeader.vue";
import Photos from "./components/photos/Photos.vue";

export default defineComponent({
  name: "App",
  components: {
    TheHeader,
    Photos,
  },
});
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap");

* {
  box-sizing: border-box;
}

html {
  font-family: "Roboto", sans-serif;
}

body {
  margin: 0;
}
</style>
