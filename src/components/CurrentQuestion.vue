<template>
  <p>{{ msg }}</p>
  <!--  <p>{{ question }}</p> We can pass props directly to UI from defineProps-->
  <p>{{ watchedQuestion }}</p>
</template>

<script setup lang="ts">
import { defineComponent, defineProps, ref, watchEffect } from "vue";

interface Props {
  msg: string;
  question: string;
}

const props = defineProps<Props>();

// We could just pass the question prop directly to the UI, however...

/**
 * Watch Effect Example
 */

const watchedQuestion = ref(props.question);
watchEffect(() => (watchedQuestion.value = props.question));

// Use when code needs to be executed automatically whenever reactive dependencies change.

// Usage:
// 1. Updating computed properties when dependencies change
// 2. Re-fetch data asynchronously when dependencies change
// 3. Modify the DOM whenever data changes
// 4. Logging reactive data when it changes (ie debugging)
</script>
