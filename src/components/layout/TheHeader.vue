<template>
  <header>
    <h1>
      {{ title }}
    </h1>
  </header>
</template>
<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  setup() {},
  props: ["title"],
});
</script>

<style scoped>
header {
  width: 100%;
  height: 5rem;
  background-color: rgb(13, 5, 67);
  display: flex;
  justify-content: center;
  align-items: center;
}

header h1 {
  color: white;
  margin: 0;
}
</style>
