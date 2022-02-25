<template>
  <div class="questions">
    <p>{{ result.question }}</p>
    <label v-for="(answers, index) in result.answers" :key="index" :for="index">
      <input
        @click="answerQuestion(answers)"
        :id="index"
        type="radio"
        :value="index"
        :disabled="selectedAnswer"
      />
      {{ answers }}
    </label>
    <button v-show="selectedAnswer" type="button" class="nextBtn">
      Next <i class="fa-solid fa-angle-right"></i>
    </button>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref, onMounted } from "vue";
const result = ref({
  question: null,
  answers: [],
});
const correctAnswer = ref(null);
const correctAnswerCount = ref(0);
const incorrentAnswerCount = ref(0);
const selectedAnswer = ref(false);
const correctAnswerCheck = ref(false);

const getQuestion = () => {
  axios.get("https://opentdb.com/api.php?amount=1").then((get_response) => {
    result.value.question = get_response.data.results[0].question;
    get_response.data.results[0].incorrect_answers.forEach((element) => {
      result.value.answers.push(element);
    });
    result.value.answers.push(get_response.data.results[0].correct_answer);
    correctAnswer.value = get_response.data.results[0].correct_answer;
  });
};

onMounted(() => {
  getQuestion();
});

const answerQuestion = (answers) => {
  selectedAnswer.value = true;
  if (answers == correctAnswer.value) {
    correctAnswerCount.value++;
    correctAnswerCheck.value = true;
    return;
  }
  incorrentAnswerCount.value++;
};
</script>

<style lang="scss">
* {
  --tw-ring-offset-shadow: 4px 4px #0000;
  --tw-ring-shadow: 4px 4px #0000;
}

.questions {
  width: 600px;
  max-width: 650px;
  background-color: #fff;
  padding: 40px;
  display: flex;
  flex-direction: column;
  gap: 10px;
  border: none;
  border-radius: 10px;
  --tw-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1),
    0 4px 6px -2px rgba(0, 0, 0, 0.05);
  box-shadow: var(--tw-ring-offset-shadow, 4px 4px #0000),
    var(--tw-ring-shadow, 4px 4px #0000), var(--tw-shadow);

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

    &:hover {
      background-color: rgb(240, 240, 240);
    }

    input {
      display: none;
    }
  }

  .nextBtn {
    cursor: pointer;
    width: 15%;
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
  border: 1px solid lightgray;
  margin: 5px 0;
  border-radius: 8px;
  padding: 11px 21px;
  background-color: rgb(212, 158, 158);

  &:hover {
    background-color: rgb(192, 141, 141);
  }

  input {
    display: none;
  }
}

.correctAnswer {
  border: 1px solid lightgray;
  margin: 5px 0;
  border-radius: 8px;
  padding: 11px 21px;
  background-color: rgb(91, 148, 100);

  &:hover {
    background-color: rgb(75, 121, 82);
  }

  input {
    display: none;
  }
}
</style>