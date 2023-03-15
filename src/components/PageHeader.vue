<template>
  <img :src="img_url" />
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "PageHeader",
  props: {
    img_url: String,
  },
});
</script>
