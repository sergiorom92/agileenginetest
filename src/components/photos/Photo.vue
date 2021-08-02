<template>
  <base-dialog v-if="inputIsInvalid" title="Picture Detail" @close="closeDialog"
    ><template #default>
      <p v-if="!pictureDetail">Loading...</p>
      <div v-else>
        <img class="full-picture" :src="pictureDetail.full_picture" />
        <p>Author: {{ pictureDetail.author }}</p>
        <p>Camera: {{ pictureDetail.camera }}</p>
        <p>Tags: {{ pictureDetail.tags }}</p>
      </div>
    </template>
    <template #actions>
      <base-button @click="closeDialog">Ok</base-button>
    </template></base-dialog
  >
  <li>
    <img :src="src" @click="openDialog()" />
  </li>
</template>
<script lang="ts">
import { defineComponent } from "vue";
import BaseButton from "../UI/BaseButton.vue";

export default defineComponent({
  components: { BaseButton },
  data() {
    return { inputIsInvalid: false, pictureDetail: null };
  },
  setup() {},
  props: ["src", "id", "token"],
  methods: {
    closeDialog() {
      this.inputIsInvalid = false;
    },
    openDialog() {
      this.inputIsInvalid = true;
      if (!this.pictureDetail) {
        this.loadImageDetail();
      }
    },
    async loadImageDetail() {
      const apiTokenReq = await fetch(
        `http://interview.agileengine.com/images/${this.id}`,
        {
          method: "GET",
          headers: {
            "Content-Type": "application/json",
            Authorization: `Bearer ${this.token}`,
          },
        }
      );
      if (200 !== apiTokenReq.status) {
        console.log("Estoy Bravo");
        return null;
      }
      this.pictureDetail = await apiTokenReq.json();
    },
  },
});
</script>

<style scoped>
li {
  height: 40vh;
  flex-grow: 1;
}

img {
  max-height: 100%;
  min-width: 100%;
  object-fit: cover;
  vertical-align: bottom;
  padding: 5px;
}
.full-picture {
  width: 0px;
}
</style>
