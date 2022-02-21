<template>
  <div class="home">
    <Carousel :sliderImg="sliderImg" class="carousel" v-slot="{ currentSlide }">
      <Slide v-for="(slider, index) in sliderImg" :key="index">
        <div v-show="currentSlide === index + 1" class="slide-info">
          <!-- <h3>{{ message }}</h3> -->
          <img :src="require(`@/assets/${slider}.jpg`)" />
        </div>
      </Slide>
    </Carousel>
  </div>
</template>

<script setup>
import Carousel from "@/components/Carousel.vue";
import Slide from "@/components/Slide.vue";
import { ref } from "vue";

const sliderImg = ref(["bg-1", "bg-2", "bg-3"]);

// const message = computed(() => {
//   if (currentSlide === 1) {
//     return "First Slide";
//   } else if (currentSlide === 2) {
//     return "Second Slide";
//   } else {
//     return "Third Slide";
//   }
// });
</script>

<style lang="scss">
.carousel {
  max-height: 100vh;
  height: 100vh;
  position: relative;

  .slide-info {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    max-height: 100%;
    height: 100%;

    img {
      min-width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }
}
</style>
