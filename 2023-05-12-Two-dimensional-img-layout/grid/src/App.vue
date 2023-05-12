<script setup lang="ts">
</script>

<template>
  <div class="container">
    <div class="img_container" v-for="i in 7">
      <img :src="`${i}.jpg`" alt="">
    </div>
  </div>
</template>

<style>
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
  gap: 4px;
}
img {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: cover;
}
</style>