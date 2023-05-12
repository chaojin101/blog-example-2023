<script setup lang="ts">
import { onMounted } from 'vue';
const totalImgAmount = 8;
const imgBaseWidth = 100;
const gap = 4;
const screenWidth = document.body.offsetWidth;
const rowImgAmount = Math.floor(screenWidth / (imgBaseWidth + gap));
const lastRowImgAmount = totalImgAmount % rowImgAmount;

const setLastRowImg = () => {
  console.log(lastRowImgAmount);
  if (lastRowImgAmount) {
    const img_containers =
      document.querySelectorAll<HTMLDivElement>(`.img_container`);
    for (
      let i = totalImgAmount - lastRowImgAmount;
      i < totalImgAmount;
      i++
    ) {
      if (img_containers[i]) {
        const imgWidth = img_containers[0].offsetWidth
        img_containers[i].style.flex = `0 ${imgWidth}px`;
      }
    }
  }
};

onMounted(setLastRowImg);
</script>

<template>
  <div class="container">
    <!-- note: v-for starts at 1 include last index -->
    <div class="img_container" v-for="i in totalImgAmount">
      <img :src="`${i}.jpg`" alt="" />
    </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-flow: wrap;
  justify-content: center;
  gap: 4px;
}
.img_container {
  flex: 1 100px;
  aspect-ratio: 3/4;
}

img {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: cover;
}
</style>
