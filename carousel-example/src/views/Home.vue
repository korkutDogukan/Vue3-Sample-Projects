<template>
  <div class="home">
    <Carousel class="carousel" v-slot="{ currentSlide }">
      <Slide v-for="(slide, index) in carouselSlides" :key="index">
        <div v-show="currentSlide === index + 1" class="slide-info">
          <img :src="require(`../assets/${slide}.jpg`)" alt="" />
        </div>
      </Slide>
    </Carousel>
  </div>
</template>

<script setup>
import Carousel from "@/components/Carousel.vue";
import Slide from "@/components/Slide.vue";
import { ref } from "vue";
const carouselSlides = ref(["bg-1", "bg-2", "bg-3"]);
</script>

<style lang="scss">
.carousel {
  position: relative;
  max-height: 100vh;
  height: 100vh;

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