<template>
  <div class="app">
    <Menu></Menu>
    <router-view />
  </div>
</template>

<script setup>
import Menu from "./components/Menu.vue";
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Josefin+Sans:wght@300;400;500;700&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Josefin Sans", sans-serif;
}

.app {
  min-height: 100vh;
  position: relative;
}
</style>
