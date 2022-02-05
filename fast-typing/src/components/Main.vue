<template>
  <div class="jumbotron container bg-success p-2 text-dark bg-opacity-10 p-5">
    <h1 class="display-4 mt-4">Hızlı Yazma Yarışması</h1>
    <p class="lead">Ne kadar hızlı klavye kullandığını test et</p>
    <hr class="my-4" />

    <div v-if="isFinish" class="alert alert-primary">
      <h3>Süre Bitti</h3>
      <p class="display-4">{{ dks }} DKS</p>
      <span>Doğruluk Yüzdesi : %{{ percentofTrue }}</span
      ><br />
      <span>Doğru Kelime : {{ trueCount }}</span
      ><br />
      <span>Yanlış Kelime : {{ falseCount }}</span>
      <br />

      <button @click="newGame" class="btn btn-success mt-4">Yeni Oyun</button>
    </div>

    <div v-else>
      <div class="card">
        <div class="card-body">
          <span
            v-for="(word, key) in words.filter((data, index) => index < 16)"
            :key="key"
            :class="key == 0 ? writingWordControl : null"
            class="words me-2"
          >
            {{ word }}
          </span>
        </div>
      </div>
      <div class="card">
        <div class="card-body bg-secondary">
          <div class="input-group input-group-lg">
            <input type="text" class="form-control" v-model="writingWord" />
            <button class="btn btn-light" type="button" disabled>
              {{ timer }} sn
            </button>
            <button
              :disabled="isRunning"
              class="btn btn-light"
              type="button"
              @click="getWords"
            >
              Yenile
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import wordList from "@/assets/words.json";
import { ref, watch, computed } from "vue";

const words = ref([]);
const writingWord = ref(null);
const isTrue = ref(true);
const trueCount = ref(0);
const falseCount = ref(0);
const timer = ref(2);
const interval = ref(false);
const isRunning = ref(false);
const isFinish = ref(false);

watch(writingWord, (val) => {
  if (!val || val === " ") {
    writingWord.value = "";
    return;
  }
  if (!isRunning.value) {
    toggleTimer();
  }
  const word = words.value[0].slice(0, val.length);
  const userWord = val.replace(" ", "");
  isTrue.value = word.toLowerCase() === userWord.toLowerCase();

  if (val.indexOf(" ") !== -1) {
    isTrue.value ? trueCount.value++ : falseCount.value++;
    words.value.splice(0, 1);
    writingWord.value = "";
    isTrue.value = true;
  }
});

const writingWordControl = computed(() => {
  return isTrue.value ? "writing-word" : "writing-word bg-danger";
});

const toggleTimer = () => {
  isRunning.value = true;
  interval.value = setInterval(timeProcess, 1000);
};

const timeProcess = () => {
  if (timer.value === 0) {
    clearInterval(interval.value);
    isFinish.value = true;
    return;
  } else {
    timer.value--;
  }
};

const getWords = () => {
  words.value = wordList.sort(() => Math.random() - 0.5).splice(0, 300);
};

getWords();

const percentofTrue = computed(() => {
  const totalWord = trueCount.value + falseCount.value;
  const percent = (100 / (totalWord / trueCount.value)).toFixed();
  return isNaN(percent) ? 0 : percent;
});

const dks = computed(() => {
  return 300 - words.value.length;
});

const newGame = () => {
  getWords();
  isFinish.value = false;
  timer.value = 60;
  isTrue.value = true;
  isRunning.value = false;
};
</script>

<style>
.words {
  font-size: 25px;
  font-weight: 400;
}

.writing-word {
  background-color: slategray;
  color: white;
  padding: 5px;
  border-radius: 5px;
}
</style>