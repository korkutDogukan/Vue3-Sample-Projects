<template>
  <div class="container">
    <div class="header">
      <div v-if="isFinish" class="resultPage">
        <h1>Game Over</h1>
        <p>Correct Word : {{ correctWord }}</p>
        <p>
          Wrong Word :
          <span v-if="wrongWord == 0 && correctWord != 0"
            >{{ wrongWord }} (+50 Points)</span
          >
          <span v-else>{{ wrongWord }}</span>
        </p>
        <p>
          Number of Letters Received :
          <span v-if="getLetters == 0"> {{ getLetters }}</span>
          <span v-else> {{ getLetters }} (-{{ letterPoint }} Point)</span>
        </p>
        <p style="font-size: 40px">
          <b>Game Score : {{ score }}</b>
        </p>
        <div class="rules">
          <h3>Score Calculation</h3>
          <span
            >The score is calculated based on the ratio of the number of correct
            <br />
            answers to the number of incorrect answers. If the wrong number is
            <br />
            0, you get +50 points. You lose -15 points for every word
            taken.</span
          >
        </div>
        <div class="startDiv">
          <button class="startBtn" @click="playAgain">Play Again</button>
        </div>
      </div>
      <div v-else>
        <h1 class="title">Word Find Game</h1>
        <p class="description">Guess The Correct Word</p>
        <div v-if="isStart == false" class="startDiv">
          <button class="startBtn" @click="isStart = true">Start</button>
        </div>
        <div v-else>
          <hr />
          <div class="guessWord">
            <span v-for="i in splitedWord" :key="i">{{ i }}</span>
          </div>
          <div class="hintWord">
            <p class="hint">
              Hint ({{ letterCount }} Harf - {{ spaceCount }} Kelime ) :
              {{ word.hint }}
            </p>
          </div>
          <hr />
          <div class="mainDiv">
            <div class="inputGroup">
              <input
                @keydown.enter="guess"
                type="text"
                class="enterWord"
                placeholder="Your Guess..."
                v-model="userGuess"
                :class="checkWord"
              />
              <span class="time">{{ time }} <i class="far fa-clock"></i></span>
            </div>
            <div class="btnGroup">
              <button @click="guess" class="guessBtn">Guess</button>
              <button @click="getLetter" class="letterBtn">Letter</button>
              <button @click="pass" class="letterBtn">Pass</button>
            </div>
            <button @click="playAgain" class="newGameBtn">New Game</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import axios from "axios";
import { ref, computed } from "vue";

const word = ref({
  text: "",
  hint: "",
});

// const usedNumber = ref([]);
const splitedWord = ref([]);
const correctWord = ref(0);
const wrongWord = ref(0);
const userGuess = ref("");
const isTrue = ref(null);
const isStart = ref(false);
const isFinish = ref(false);
const time = ref(60);
const myInterval = ref(null);
const letterPoint = ref(0);
const getLetters = ref(0);

const getWord = () => {
  const rondomNumb = ref(Math.random() * 10000);
  axios
    .get(
      "https://raw.githubusercontent.com/bilalozdemir/tr-word-list/master/files/words.json"
    )
    .then((response) => {
      if (response.data[Math.round(rondomNumb.value)].word.length > 20) {
        return getWord();
      } else {
        word.value.text = response.data[Math.round(rondomNumb.value)].word;
        word.value.hint =
          response.data[Math.round(rondomNumb.value)].meanings[0];
      }
      // splitedWord.value = word.value.text.split("");
    });
};

getWord();

myInterval.value = setInterval(() => {
  if (time.value == 0) {
    isFinish.value = true;
    clearInterval(myInterval);
  } else {
    if (isStart.value) {
      time.value--;
    }
  }
}, 1000);

const usedIndexes = ref([]);
const randomNumber = ref(null);

function getUniqueRandomNumber(x) {
  const index = Math.floor(Math.random() * x);
  if (usedIndexes.value.includes(index)) {
    return getUniqueRandomNumber(x);
  } else {
    usedIndexes.value.push(index);
    randomNumber.value = index;
  }
}
const countLimit = ref(0);

const getLetter = () => {
  countLimit.value++;
  if (countLimit.value <= word.value.text.length) {
    letterPoint.value = letterPoint.value + 15;
    getLetters.value++;
    getUniqueRandomNumber(word.value.text.length);
    // const rondomNumb2 = ref(Math.floor(Math.random() * word.value.text.length));
    splitedWord.value[randomNumber.value] =
      word.value.text.split("")[randomNumber.value];
  } else {
    userGuess.value = word.value.text;
    alert("Daha fazla kelime alamazsınız!!");
  }
};

const guess = () => {
  // const rondomNumb = ref(Math.random() * 10000);
  const replacedUserGuess = userGuess.value.replace(/ +/g, "");
  const repolacedWordText = word.value.text.replace(/ +/g, "");
  if (replacedUserGuess.toLowerCase() == repolacedWordText.toLowerCase()) {
    correctWord.value++;
    isTrue.value = true;
    // usedNumber.value = [];
    countLimit.value = 0;
    usedIndexes.value = [];
    splitedWord.value = [];
    setTimeout(() => {
      userGuess.value = "";
      getWord();
    }, 600);
    setTimeout(() => {
      isTrue.value = null;
    }, 1200);
  } else {
    wrongWord.value++;
    isTrue.value = false;
    // usedNumber.value = [];
    countLimit.value = 0;
    usedIndexes.value = [];
    splitedWord.value = [];
    userGuess.value = word.value.text;
    setTimeout(() => {
      userGuess.value = "";
      getWord();
    }, 600);
    setTimeout(() => {
      isTrue.value = null;
    }, 1200);
  }
};

const checkWord = computed(() => {
  if (isTrue.value === null) {
    return "enterWord";
  } else if (isTrue.value === true) {
    return "correct";
  } else {
    return "wrong";
  }
});

const playAgain = () => {
  // const rondomNumb = ref(Math.random() * 10000);
  setTimeout(() => {
    splitedWord.value = [];
    // usedNumber.value = [];
    countLimit.value = 0;
    usedIndexes.value = [];
    correctWord.value = 0;
    wrongWord.value = 0;
    letterPoint.value = 0;
    getLetters.value = 0;
    passCount.value = 0;
    isTrue.value = null;
    isStart.value = false;
    isFinish.value = false;
    time.value = 60;
    userGuess.value = "";
    getWord();
  }, 400);
};

const score = computed(() => {
  if (wrongWord.value == 0 && correctWord.value == 0) {
    return 0;
  } else {
    if (wrongWord.value == 0) {
      return ((correctWord.value / 1) * 100 - letterPoint.value + 50).toFixed(
        2
      );
    } else {
      return (
        (correctWord.value / wrongWord.value) * 100 -
        letterPoint.value
      ).toFixed(2);
    }
  }
});

const passCount = ref(0);

const pass = () => {
  passCount.value++;
  if (passCount.value > 3) {
    alert("En fazla 3 kere pas geçebilirsiniz!!!");
  } else {
    countLimit.value = 0;
    usedIndexes.value = [];
    splitedWord.value = [];
    userGuess.value = "";
    getWord();
  }
};

const spaceCount = computed(() => {
  const deneme = word.value.text.split("");
  const spaceCount = ref(0);
  for (let index = 0; index < deneme.length; index++) {
    const element = deneme[index];
    if (element == " ") {
      spaceCount.value++;
    }
  }
  return spaceCount.value + 1;
});

const letterCount = computed(() => {
  const count = word.value.text.replace(/ +/g, "");
  return count.length;
});
</script>
