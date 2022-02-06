<template>
  <div class="container">
    <div class="header">
      <h1 class="title">Word Find Game</h1>
      <p class="description">Guess The Correct Word</p>
      <hr />
      <div class="guessWord">
        <span v-for="i in splitedWord" :key="i">{{ i }}</span>
      </div>
      <div class="hintWord">
        <p class="hint">Hint : {{ word.hint }}</p>
      </div>
      <hr />
      <div class="mainDiv">
        <div class="inputGroup">
          <input type="text" class="enterWord" placeholder="Your Guess..." />
          <span class="time">60 <i class="far fa-clock"></i></span>
        </div>
        <div class="btnGroup">
          <button class="guessBtn">Guess</button>
          <button @click="getLetter" class="letterBtn">Letter</button>
        </div>
        <button class="newGameBtn">New Game</button>
      </div>
    </div>
  </div>
</template>
<script setup>
import axios from "axios";
import { ref } from "vue";

const rondomNumb = ref(Math.random() * 10000);
const word = ref({
  text: "",
  hint: "",
});

const splitedWord = ref([]);
axios
  .get(
    "https://raw.githubusercontent.com/bilalozdemir/tr-word-list/master/files/words.json"
  )
  .then((response) => {
    word.value.text = response.data[Math.round(rondomNumb.value)].word;
    word.value.hint = response.data[Math.round(rondomNumb.value)].meanings[0];

    // splitedWord.value = word.value.text.split("");
  });

const getLetter = () => {
  const rondomNumb2 = Math.round(Math.random() * word.value.text.length);
  splitedWord.value[rondomNumb2] = word.value.text.split("")[rondomNumb2];
  console.log(word.value.text.split("")[rondomNumb2]);
  console.log(splitedWord.value);
  console.log(word.value.text);
};
</script>
