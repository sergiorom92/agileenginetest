<template>
  <teleport to="body">
    <div @click="$emit('close')"></div>
    <dialog open>
      <header>
        <slot name="header"
          ><h2>{{ title }}</h2> <img @click="$emit('close')" src="src/assets/close.svg"/></slot
        >
      </header>
      <section>
        <slot></slot>
      </section>
      <menu>
        <slot name="actions">
          <base-button @click="$emit('close')">Close</base-button>
        </slot>
      </menu>
    </dialog>
  </teleport>
</template>
<script lang="ts">
import { defineComponent } from "vue";
import BaseButton from "./BaseButton.vue";

export default defineComponent({
  components: { BaseButton },
  setup() {},
  props: { title: { type: String, required: false } },
  emits: ["close"],
});
</script>
<style scoped>
h2 {
  display: inline-block;
}
div {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.75);
  z-index: 10;
}

dialog {
  position: fixed;
  width: 80%;
  top: 50%; 
  left: 50%; transform: translate(-50%, -50%);
  z-index: 100;
  border-radius: 12px;
  border: none;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 0;
  margin: 0;
  overflow: hidden;
}

header {
  background-color: #3a0061;
  color: white;
  width: 100%;
  padding: 1rem;
}

header h2 {
  margin: 0;
}

section {
  display: relative;
  padding: 1rem;
}

menu {
  padding: 1rem;
  display: flex;
  justify-content: flex-end;
  margin: 0;
}
@media(min-width: 768px){
  dialog  {
    width: 40%;
  }
}

img{
  width: 2rem;
  filter: invert(1);
  float: right;
}

</style>
