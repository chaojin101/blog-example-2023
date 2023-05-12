<script setup lang="ts">
import { onMounted } from 'vue';
const totalImgAmount = 8;
const imgBaseWidth = 100;
const screenWidth = document.body.offsetWidth;
const rowImgAmount = Math.floor(screenWidth / imgBaseWidth);
const lastRowImgAmount = totalImgAmount % rowImgAmount;
const rowAmount =
  Math.floor(totalImgAmount / rowImgAmount) + (lastRowImgAmount ? 1 : 0);
const imgWidth = Math.floor(screenWidth / rowImgAmount); // used for last row's img

onMounted(() => {
  if (lastRowImgAmount) {
    const img_containers = document.querySelectorAll<HTMLDivElement>(
      '.flex_row_container:last-child .img_container'
    );
    console.log(img_containers)
    for (let img_container of img_containers) {
      img_container.style.flex = `0 ${imgWidth}px`;
    }
  }
});
</script>

<template>
  <div class="container">
    <!-- note: v-for starts at 1 include last index -->
    <div class="flex_row_container" v-for="rowIndex in rowAmount">
      <!-- this is not the last row -->
      <template v-if="rowIndex !== rowAmount">
        <div class="img_container" v-for="j in rowImgAmount">
          <img :src="`${(rowIndex - 1) * rowImgAmount + j}.jpg`" alt="" />
        </div>
      </template>
      <!-- this is the last row -->
      <template v-else-if="rowIndex === rowAmount">
        <div class="img_container" v-for="j in lastRowImgAmount">
          <img :src="`${(rowIndex - 1) * rowImgAmount + j}.jpg`" alt="" />
        </div>
      </template>
    </div>
  </div>
</template>

<style scoped>
.container {
  /* using flexbox to control flex_row_container gap */
  display: flex;
  flex-flow: column;
  gap: 4px;
}

.flex_row_container {
  display: flex;
  gap: 4px;
  justify-content: center;
}
.flex_row_container .img_container {
  flex: 1 100px;
}

img {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: cover;
}
</style>
