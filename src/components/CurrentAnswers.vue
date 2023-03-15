<template>
  <p>{{ msg }}</p>
  <ul class="answer-container">
    <li
      v-for="(answer, i) in answers"
      :key="i"
      class="answer"
      :class="{
        green: answer.correct && answer.clicked,
        red: !answer.correct && answer.clicked,
      }"
      @click="clickAnswer(answer)"
    >
      {{ answer.text }}
    </li>
  </ul>
</template>

<script setup lang="ts">
import { defineComponent, defineProps } from "vue";

// Ex: Without interface

// const props = defineProps({
//   msg: { type: String, required: true },
//   answers: { type: Array, required: true },
//   answerQuestion: { type: Function, required: true },
// });

interface Answer {
  text: string;
  correct: boolean;
  clicked: boolean;
}

interface Props {
  msg: string;
  answers: Answer[];
  answerQuestion: (answer: Answer) => void;
}

const props = defineProps<Props>();

const clickAnswer = (answer: Answer) => {
  props.answerQuestion(answer);
  answer["clicked"] = true;
};
</script>

<style scoped>
.answer-container {
  display: flex;
  text-decoration: none;
  flex-direction: row;
  gap: 2rem;
  list-style: none;
  max-width: 1100px;
  min-height: 300px;
}

.answer {
  padding: 2rem;
  border: 1px solid black;
  max-width: 350px;
  width: 100%;
  background-color: #bdbdbd;
  color: #252525;
  border-radius: 8px;
}

.red {
  background-color: #f44336;
}

.green {
  background-color: #4caf50;
}
</style>
