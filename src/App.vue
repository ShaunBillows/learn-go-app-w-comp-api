<template>
  <PageHeader img_url="/assets/logo.png" />
  <p v-if="error">An error occurred connecting to the server.</p>
  <div v-else>
    <CurrentQuestion msg="" :question="question" />
    <CurrentAnswers
      msg=""
      :answers="answers"
      :getQuestion="getQuestion"
      :answerQuestion="answerQuestion"
    />
    <p>{{ score }}</p>
    <button @click="getQuestion">Next Question</button>
  </div>
</template>

<script setup lang="ts">
import {
  defineComponent,
  ref,
  computed,
  onMounted,
  toRefs,
  reactive,
} from "vue";
import PageHeader from "./components/PageHeader.vue";
import CurrentQuestion from "./components/CurrentQuestion.vue";
import CurrentAnswers from "./components/CurrentAnswers.vue";
import { fetchRandomQuestion } from "@/Services/questionService";

interface Answer {
  text: string;
  correct: boolean;
}

interface Data {
  id: number;
  question: string;
  answer: string;
  incorrect_answer_1: string;
  incorrect_answer_2: string;
  difficulty: number;
  topic: string;
  error: false;
}

/**
 * Composition API w/ setup script tag and reactive syntax
 */

const state = reactive({
  answers: [] as Answer[],
  score: 0,
  questionAttempted: false,
  error: false,
  data: {} as Data,
});

const getQuestion = async () => {
  state.questionAttempted = false;
  state.data = await fetchRandomQuestion();
  if (state.data.error) {
    state.error = true;
  }
  state.answers = [
    { text: state.data.answer, correct: true },
    { text: state.data.incorrect_answer_1, correct: false },
    { text: state.data.incorrect_answer_2, correct: false },
  ].sort(() => Math.random() - 0.5);
};
const answerQuestion = (answer: Answer) => {
  // increments the score if answered correctly first time
  if (answer.correct && !state.questionAttempted) {
    state.score++;
  }
  state.questionAttempted = true;
};

const question = computed(() => state.data.question);

// toRefs: destructures keys from a reactive state for direct use in template
const { answers, score } = toRefs(state);

onMounted(() => getQuestion());

/**
 * Composition API w/ setup script tag and ref syntax
 */

// const answers = ref([] as Answer[]);
// const score = ref(0);
// const questionAttempted = ref(false);
// const error = ref(false);
// const data = ref<Data>({
//   id: 0,
//   question: "",
//   answer: "",
//   incorrect_answer_1: "",
//   incorrect_answer_2: "",
//   difficulty: 0,
//   topic: "",
//   error: false,
// });
// const getQuestion = async () => {
//   questionAttempted.value = false;
//   data.value = await fetchRandomQuestion();
//   if (data.value.error) {
//     error.value = true;
//   }
//   answers.value = [
//     { text: data.value.answer, correct: true },
//     { text: data.value.incorrect_answer_1, correct: false },
//     { text: data.value.incorrect_answer_2, correct: false },
//   ].sort(() => Math.random() - 0.5);
// };
// const answerQuestion = (answer: Answer) => {
//   // increments the score if answered correctly first time
//   if (answer.correct && !questionAttempted.value) {
//     score.value++;
//   }
//   questionAttempted.value = true;
// };
//
// const question = computed(() => data.value.question);
//
// onMounted(() => getQuestion());

/**
 * Composition API w/ setup method
 */

// export default defineComponent({
//   name: "App",
//   components: { PageHeader, CurrentQuestion, CurrentAnswers },
//   setup() {
//     // const question = ref("");
//     const answers = ref([] as Answer[]);
//     const score = ref(0);
//     const questionAttempted = ref(false);
//     const error = ref(false);
//     const data = ref<Data>({
//       id: 0,
//       question: "",
//       answer: "",
//       incorrect_answer_1: "",
//       incorrect_answer_2: "",
//       difficulty: 0,
//       topic: "",
//       error: false,
//     });
//     const getQuestion = async () => {
//       questionAttempted.value = false;
//       data.value = await fetchRandomQuestion();
//       if (data.value.error) {
//         error.value = true;
//       }
//       answers.value = [
//         { text: data.value.answer, correct: true },
//         { text: data.value.incorrect_answer_1, correct: false },
//         { text: data.value.incorrect_answer_2, correct: false },
//       ].sort(() => Math.random() - 0.5);
//     };
//     const question = computed(() => data.value.question);
//     const answerQuestion = (answer: Answer) => {
//       // increments the score if answered correctly first time
//       if (answer.correct && !questionAttempted.value) {
//         score.value++;
//       }
//       questionAttempted.value = true;
//     };
//     return {
//       answers,
//       score,
//       questionAttempted,
//       error,
//       data,
//       getQuestion,
//       answerQuestion,
//       question,
//     };
//   },
//   mounted() {
//     this.getQuestion();
//   },
// });

/**
 * Options API
 */

// export default defineComponent({
//   name: "App",
//   components: { PageHeader, CurrentQuestion, CurrentAnswers },
//   data() {
//     return {
//       answers: [] as Answer[],
//       score: 0,
//       questionAttempted: false,
//       error: false,
//       data: {
//         id: "",
//         question: "",
//         answer: "",
//         incorrect_answer_1: "",
//         incorrect_answer_2: "",
//         difficulty: 0,
//         topic: "",
//         error: false,
//       },
//     };
//   },
//   methods: {
//     async getQuestion() {
//       this.questionAttempted = false;
//       this.data = await fetchRandomQuestion();
//       if (this.data.error) {
//         this.error = true;
//       }
//       this.answers = [
//         { text: this.data.answer, correct: true },
//         { text: this.data.incorrect_answer_1, correct: false },
//         { text: this.data.incorrect_answer_2, correct: false },
//       ].sort(() => Math.random() - 0.5);
//     },
//     answerQuestion(answer: Answer) {
//       // increments the score if answered correctly first time
//       if (answer.correct && !this.questionAttempted) {
//         this.score++;
//       }
//       this.questionAttempted = true;
//     },
//   },
//   computed: {
//     question(): string {
//       return this.data.question;
//     },
//   },
//   mounted() {
//     this.getQuestion();
//   },
// });
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #f5f5f5;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
  font-size: 1.3em;
}

body {
  background-color: #252525;
}
</style>
