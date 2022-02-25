<template>
  <div class="home">
    <h1 class="title">Simple Quiz</h1>
    <Questions></Questions>
  </div>
</template>

<script setup>
import Questions from "@/components/Questions.vue";
</script>

<style lang="scss">
.home {
  width: 100%;
  height: 100vh;
  background-color: rgb(230, 227, 227);
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;

  .title {
    font-size: 45px;
    font-weight: 900;
    color: rgb(182, 63, 42);
    margin: 20px 0;
  }
}
</style>
