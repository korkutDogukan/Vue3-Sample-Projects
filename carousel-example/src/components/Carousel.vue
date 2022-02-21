<template>
  <div class="carousel">
    <slot :currentSlide="currentSlide"></slot>

    <div class="navigate">
      <div class="toggle-page left">
        <i @click="prevSlide" class="fas fa-chevron-left"></i>
      </div>
      <div class="toggle-page right">
        <i @click="nextSlide" class="fas fa-chevron-right"></i>
      </div>
    </div>

    <div class="pagination">
      <span
        v-for="(pagi, index) in props.sliderImg.length"
        :key="index"
        :class="{ active: currentSlide === index + 1 }"
        @click="goSlide(index)"
      ></span>
    </div>
  </div>
</template>

<script setup>
import { ref, defineProps } from "vue";
const props = defineProps(["sliderImg"]);
const currentSlide = ref(1);
const router = ref(true);
const routerTime = ref(3000);

const prevSlide = () => {
  router.value = false;
  if (currentSlide.value < 2) {
    currentSlide.value = 3;
    return;
  } else {
    currentSlide.value--;
    return;
  }
};

const nextSlide = () => {
  router.value = false;
  if (currentSlide.value >= props.sliderImg.length) {
    currentSlide.value = 1;
    return;
  } else {
    currentSlide.value++;
    return;
  }
};

const goSlide = (index) => {
  currentSlide.value = index + 1;
};

setInterval(() => {
  if (router.value) {
    if (currentSlide.value >= props.sliderImg.length) {
      currentSlide.value = 1;
      return;
    } else {
      currentSlide.value++;
      return;
    }
  }
  router.value = true;
}, routerTime.value);
</script>


<style lang="scss">
.navigate {
  padding: 0 16px;
  height: 100%;
  width: 100%;
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;

  .toggle-page {
    display: flex;
    flex: 1;

    i {
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 50%;
      width: 40px;
      height: 40px;
      border: 1px solid #fff;
      background-color: rgba(56, 133, 136, 0.8);
      color: #fff;
    }
  }

  .right {
    justify-content: flex-end;
  }
}

.pagination {
  position: absolute;
  bottom: 24px;
  width: 100%;
  display: flex;
  gap: 16px;
  justify-content: center;
  align-items: center;

  span {
    cursor: pointer;
    width: 20px;
    height: 20px;
    border: 1px solid #fff;
    border-radius: 50%;
    background-color: rgba(56, 133, 136, 0.8);
    box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
  }

  .active {
    background-color: rgba(209, 221, 221, 0.8);
  }
}
</style>