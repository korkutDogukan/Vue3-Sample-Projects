<template>
  <div>
    <div v-if="!isFinish" class="questions">
      <span class="timer">{{ currentQuestion }} / 10</span>
      <p>{{ result.question }}</p>
      <label
        :for="index"
        :class="[
          index == answerIndex ? answerStyle : null,
          index == correctIndex && selectedAnswer ? correct : null,
        ]"
        v-for="(answers, index) in result.answers"
        :key="index"
      >
        <input
          @click="answerQuestion(answers, index)"
          :id="index"
          type="radio"
          :value="index"
          :disabled="selectedAnswer"
        />
        {{ answers }}
      </label>
      <div v-show="selectedAnswer" class="infoSection">
        <button
          v-if="currentQuestion < 10"
          @click="nextBtn"
          type="button"
          class="nextBtn"
        >
          Next <i class="fa-solid fa-angle-right"></i>
        </button>
        <button v-else @click="finishBtn" type="button" class="finishBtn">
          Finish <i class="fa-solid fa-flag-checkered"></i>
        </button>
      </div>
    </div>
    <div v-else class="questions">
      <h2>Results</h2>
      <span style="color: rgb(140, 190, 148)"
        >Number of Correct Answers : {{ correctAnswerCount }}</span
      >
      <span style="color: rgb(212, 158, 158)"
        >Number of Incorrect Answers : {{ incorrentAnswerCount }}</span
      >
      <span>Accuracy Rate : % {{ accuracyRate }}</span>
      <button @click="playAgain" class="playAgainBtn">Play Again</button>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref, onMounted, computed } from "vue";
const result = ref({
  question: null,
  answers: [],
});
const correctAnswer = ref(null);
const correctAnswerCount = ref(0);
const incorrentAnswerCount = ref(0);
const selectedAnswer = ref(false);
const correctAnswerCheck = ref(null);
const answerIndex = ref(null);
const correctIndex = ref(null);
const currentQuestion = ref(1);
const isFinish = ref(false);

const getQuestion = () => {
  axios.get("https://opentdb.com/api.php?amount=1").then((get_response) => {
    let randomNumber = Math.floor(
      Math.random() *
        (get_response.data.results[0].incorrect_answers.length + 1)
    );
    correctIndex.value = randomNumber;
    console.log(randomNumber);
    result.value.question = get_response.data.results[0].question;
    get_response.data.results[0].incorrect_answers.forEach((element) => {
      result.value.answers.push(element);
    });
    result.value.answers.splice(
      randomNumber,
      0,
      get_response.data.results[0].correct_answer
    );
    correctAnswer.value = get_response.data.results[0].correct_answer;
  });
};

onMounted(() => {
  getQuestion();
});

const answerQuestion = (answers, index) => {
  selectedAnswer.value = true;
  answerIndex.value = index;
  if (answers == correctAnswer.value) {
    correctAnswerCount.value++;
    correctAnswerCheck.value = true;
    return;
  }
  incorrentAnswerCount.value++;
  correctAnswerCheck.value = false;
};

const answerStyle = computed(() => {
  if (correctAnswerCheck.value) {
    return "correctAnswer";
  } else {
    return "wrongAnswer";
  }
});

const correct = computed(() => {
  return "correctAnswer";
});

const nextBtn = () => {
  currentQuestion.value++;
  selectedAnswer.value = false;
  correctAnswerCheck.value = null;
  answerIndex.value = null;
  result.value.answers = [];
  getQuestion();
};

const finishBtn = () => {
  isFinish.value = true;
};

const accuracyRate = computed(() => {
  let totalAnswer = correctAnswerCount.value + incorrentAnswerCount.value;
  return ((correctAnswerCount.value / totalAnswer) * 100).toFixed(2);
});

const playAgain = () => {
  correctAnswer.value = null;
  correctAnswerCount.value = 0;
  incorrentAnswerCount.value = 0;
  selectedAnswer.value = false;
  correctAnswerCheck.value = null;
  answerIndex.value = null;
  currentQuestion.value = 1;
  isFinish.value = false;
  result.value.answers = [];
  getQuestion();
};
</script>

<style lang="scss">
* {
  --tw-ring-offset-shadow: 4px 4px #0000;
  --tw-ring-shadow: 4px 4px #0000;
}

.questions {
  width: 550px;
  max-width: 650px;
  background-color: #fff;
  padding: 40px;
  display: flex;
  flex-direction: column;
  position: relative;
  gap: 10px;
  border: none;
  border-radius: 10px;
  --tw-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1),
    0 4px 6px -2px rgba(0, 0, 0, 0.05);
  box-shadow: var(--tw-ring-offset-shadow, 4px 4px #0000),
    var(--tw-ring-shadow, 4px 4px #0000), var(--tw-shadow);

  .timer {
    position: absolute;
    top: 0;
    right: 2px;
    font-size: 18px;
    border-left: 2px solid rgb(182, 63, 42);
    border-bottom: 2px solid rgb(182, 63, 42);
    border-radius: 5px;
    padding: 3px;
    font-weight: 700;
  }

  p {
    font-size: 27px;
    font-weight: 700;
  }

  label {
    border: 1px solid lightgray;
    margin: 5px 0;
    border-radius: 8px;
    padding: 11px 21px;
    background-color: rgb(251, 251, 251);
    cursor: pointer;

    &:hover {
      background-color: rgb(240, 240, 240);
    }

    input {
      display: none;
    }
  }

  .infoSection {
    display: flex;
    justify-content: flex-end;
    align-items: center;

    .nextBtn {
      cursor: pointer;
      width: 15%;
      background-color: rgb(182, 63, 42);
      border: none;
      border-radius: 15px;
      padding: 8px 9px;
      color: #fff;
      font-size: 15px;

      &:hover {
        opacity: 0.95;
      }
    }

    .finishBtn {
      cursor: pointer;
      width: 20%;
      background-color: rgb(182, 63, 42);
      border: none;
      border-radius: 15px;
      padding: 8px 9px;
      color: #fff;
      font-size: 15px;

      &:hover {
        opacity: 0.95;
      }
    }
  }

  h2 {
    font-size: 28px;
    border-bottom: 2px solid #333;
    width: 50%;
  }

  span {
    font-size: 20px;
    margin: 10px 0;
  }

  .playAgainBtn {
    cursor: pointer;
    width: 25%;
    align-self: flex-end;
    background-color: rgb(182, 63, 42);
    border: none;
    border-radius: 15px;
    padding: 8px 9px;
    color: #fff;
    font-size: 15px;

    &:hover {
      opacity: 0.95;
    }
  }
}

.wrongAnswer {
  background-color: rgb(212, 158, 158) !important;

  &:hover {
    background-color: rgb(192, 141, 141) !important;
  }
}

.correctAnswer {
  background-color: rgb(140, 190, 148) !important;

  &:hover {
    background-color: rgb(116, 160, 123) !important;
  }
}
</style>