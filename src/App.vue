<script setup>
import Waterfall1 from "./components/waterfall.vue";
import { LazyImg, Waterfall } from "vue-waterfall-plugin-next";
import "vue-waterfall-plugin-next/dist/style.css";
import { ref } from "vue";

const imageData = ref([
  {
    title: "image-1",
    imgUrl: new URL("@/assets/images/1.jpg", import.meta.url).href,
  },
  {
    title: "image-2",
    imgUrl: new URL("@/assets/images/2.jpg", import.meta.url).href,
  },
  {
    title: "image-3",
    imgUrl: new URL("@/assets/images/3.jpg", import.meta.url).href,
  },
  {
    title: "image-4",
    imgUrl: new URL("@/assets/images/4.jpg", import.meta.url).href,
  },
  {
    title: "image-5",
    imgUrl: new URL("@/assets/images/5.jpg", import.meta.url).href,
  },
  {
    title: "image-6",
    imgUrl: new URL("@/assets/images/6.jpg", import.meta.url).href,
  },
  {
    title: "image-7",
    imgUrl: new URL("@/assets/images/7.jpg", import.meta.url).href,
  },
  {
    title: "image-8",
    imgUrl: new URL("@/assets/images/8.jpg", import.meta.url).href,
  },
  {
    title: "image-9",
    imgUrl: new URL("@/assets/images/9.jpg", import.meta.url).href,
  },
]);

const dataList = ref([
  {
    title: "image-1",
    src: new URL("@/assets/images/1.jpg", import.meta.url).href,
  },
  {
    title: "image-2",
    src: new URL("@/assets/images/2.jpg", import.meta.url).href,
  },
  {
    title: "image-3",
    src: new URL("@/assets/images/3.jpg", import.meta.url).href,
  },
  {
    title: "image-4",
    src: new URL("@/assets/images/4.jpg", import.meta.url).href,
  },
  {
    title: "image-5",
    src: new URL("@/assets/images/5.jpg", import.meta.url).href,
  },
  {
    title: "image-6",
    src: new URL("@/assets/images/6.jpg", import.meta.url).href,
  },
  {
    title: "image-7",
    src: new URL("@/assets/images/7.jpg", import.meta.url).href,
  },
  {
    title: "image-8",
    src: new URL("@/assets/images/8.jpg", import.meta.url).href,
  },
  {
    title: "image-9",
    src: new URL("@/assets/images/9.jpg", import.meta.url).href,
  },
]);
</script>

<template>
  <div>
    <div class="container">
      <h1 class="title">瀑布流图片展示</h1>
      <Waterfall1
        :imageData="imageData"
        :columnCount="3"
        :columnGap="30"
        :totalWidth="940"
        backgroundColor="#f0f2f5"
      />
      <h1 class="title">瀑布流图片展示22</h1>
      <Waterfall :list="dataList">
        <template #default="{ url }">
          <div class="card">
            <LazyImg :url="url" />
          </div>
        </template>
      </Waterfall>
    </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  background-color: #f5f5f5;
  min-height: 100vh;
}

.title {
  font-size: 24px;
  margin-bottom: 30px;
  color: #333;
  text-align: center;
}

.card {
  padding: 10px;
  :deep(.lazy__img) {
    border-radius: 8px;
  }
}
</style>
