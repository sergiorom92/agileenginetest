<template>
  <base-card v-if="pictures === null">
    <div>Loading...</div>
  </base-card>
  <base-card v-if="pictures && !pictures.length">
    <div>No available pictures</div>
  </base-card>
  <div style="text-align: center">
    <base-button
      @click="previousImages()"
      v-if="picIndex > 1"
      style="margin: 5px; float: left"
      >&lt;</base-button
    >
    <base-button
      @click="nextImages()"
      v-if="hasMore"
      style="margin: 5px; float: right"
      >&gt;</base-button
    >
  </div>
  <ul>
    <photo
      v-for="picture in pictures"
      :key="picture.id"
      :id="picture.id"
      :src="picture.cropped_picture"
      :token="token"
    >
    </photo>
  </ul>
</template>
<script lang="ts">
import { defineComponent, ref } from "vue";
import Photo from "./Photo.vue";

export default defineComponent({
  components: { Photo },
  setup() {},
  async created() {
    await this.getAuthToken();
    await this.getPictures();

    try {
    } catch (error) {
      console.log("Error", error);
    }
  },
  data() {
    return {
      token: null,
      pictures: null,
      picIndex: 1,
      hasMore: false,
      key: 0,
    };
  },
  methods: {
    previousImages() {
      this.picIndex = this.picIndex - 1;
      this.getPictures();
    },
    nextImages() {
      this.picIndex = this.picIndex + 1;
      this.getPictures();
    },
    async getPictures() {
      const apiTokenReq = await fetch(
        `http://interview.agileengine.com/images?page=${this.picIndex}`,
        {
          method: "GET",
          headers: {
            "Content-Type": "application/json",
            Authorization: `Bearer ${this.token}`,
          },
        }
      );
      if (200 !== apiTokenReq.status) {
        return null;
      }
      const result = await apiTokenReq.json();
      this.pictures = result.pictures;
      this.hasMore = result.hasMore;
      this.key += 1;
    },
    async getAuthToken() {
      const apiTokenReq = await fetch("http://interview.agileengine.com/auth", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ apiKey: "23567b218376f79d9415" }),
      });
      if (200 !== apiTokenReq.status) {
        console.log("Estoy Bravo");
        return null;
      }
      this.token = (await apiTokenReq.json()).token;
    },
    setSelectedTab(tab: string) {
      this.selectedTab = tab;
    },
    addResource(title: string, description: string, link: string) {
      const newResource = {
        id: new Date().toISOString(),
        title,
        description,
        link,
      };
      this.storedResources.unshift(newResource);
      this.selectedTab = "stored-resources";
    },
    removeResource(id: string) {
      const resourceIndex = this.storedResources.findIndex(
        (res) => res.id === id
      );
      this.storedResources.splice(resourceIndex, 1);
    },
  },
  computed: {
    storedResButtonMode() {
      return (this as any).selectedTab === "stored-resources" ? null : "flat";
    },
    addResButtonMode() {
      return (this as any).selectedTab === "add-resource" ? null : "flat";
    },
  },
});
</script>
<style scoped>
ul {
  display: flex;
  flex-wrap: wrap;
  list-style-type: none;
  padding: 0;
}

li {
  height: 40vh;
  flex-grow: 1;
}

li:last-child {
  flex-grow: 10;
}

img {
  max-height: 100%;
  min-width: 100%;
  object-fit: cover;
  vertical-align: bottom;
}

@media (max-aspect-ratio: 1/1) {
  li {
    height: 30vh;
  }
}

@media (max-height: 480px) {
  li {
    height: 80vh;
  }
}

@media (max-aspect-ratio: 1/1) and (max-width: 480px) {
  ul {
    flex-direction: row;
  }

  li {
    height: auto;
    width: 100%;
  }
  img {
    width: 100%;
    max-height: 75vh;
    min-width: 0;
  }
}
</style>
