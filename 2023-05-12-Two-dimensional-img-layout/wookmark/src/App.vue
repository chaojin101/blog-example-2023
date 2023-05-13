<script setup lang="ts">
import { ref, onMounted, computed } from 'vue';
const getMinItemIndex = (arr: number[]) => {
  let minItemIndex = 0;
  for (let i = 1; i < arr.length; i++) {
    if (arr[i] < arr[minItemIndex]) {
      minItemIndex = i;
    }
  }
  return minItemIndex;
};

const imgAmount = ref(8);
const containerHeight = ref(1000); // will be caculated after img loaded
const colAmount = ref(3);
const gap = ref(4);
const colWidth = computed(
  () =>
    (document.body.offsetWidth - gap.value * (colAmount.value + 1)) /
    colAmount.value
);
const imgRefs = ref<HTMLImageElement[]>([]);
const imgContainerStyles = ref<any[]>(new Array(imgAmount.value)); // calculated by imgRefs

const waitImgsLoaded = async () => {
  // wait for all the img loaded to get the right img height
  const promises = [];
  for (let i = 0; i < imgAmount.value; i++) {
    const promise = new Promise<void>((resolve) => {
      const listener = () => {
        imgRefs.value[i].removeEventListener('load', listener);
        resolve();
      };
      imgRefs.value[i].addEventListener('load', listener);
    });
    promises.push(promise);
  }
  await Promise.all(promises);
};

const setWookmark = async () => {
  await waitImgsLoaded();

  // caculate position
  const colHeights = new Array(colAmount.value).fill(0);
  for (let i = 0; i < imgAmount.value; i++) {
    const minHeightColIndex = getMinItemIndex(colHeights);
    const left = gap.value + minHeightColIndex * (gap.value + colWidth.value);
    const top = colHeights[minHeightColIndex] + gap.value;
    imgContainerStyles.value[i] = {
      position: 'absolute',
      left: left + 'px',
      top: top + 'px',
    };
    colHeights[minHeightColIndex] += gap.value + imgRefs.value[i].offsetHeight;
  }

  containerHeight.value = Math.max(...colHeights);
};
onMounted(async () => {
  await setWookmark();
});
</script>

<template>
  <div :style="{ height: containerHeight + 'px' }" class="container">
    <div
    v-for="i in imgAmount"
    class="img_container"
    :style="{ width: colWidth + 'px', ...imgContainerStyles[i - 1] }"
    >
    <!-- this i starts at 1 -->
      <img ref="imgRefs" :src="`${i}.jpg`" alt="" />
    </div>
  </div>
</template>

<style scoped>
.container {
  position: relative;
}
img {
  display: block;
  width: 100%;
}
</style>
