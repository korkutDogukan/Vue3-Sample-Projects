<template>
  <div class="slide">
    <transition name="slide">
      <slot></slot>
    </transition>
  </div>
</template>

<script setup>
</script>

<style lang="scss">
.slide-enter-active,
.slide-leave-active {
  transition: opacity 1s ease-in-out;
}

.slide-enter-from,
.slide-leave-to {
  opacity: 0;
}

.slide-enter-to,
.slide-leave-from {
  opacity: 1;
}
</style>