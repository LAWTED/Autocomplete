<template>
  <autoComplete :value="value" :items="items" />
</template>
./components/autoComplete.vue

<script lang="ts" setup>
import autoComplete from "./components/autoComplete.vue";
import { ref } from "vue";
const value = ref("");
const items = ref<string[]>([]);
items.value = [
  "top-start",
  "top",
  "top-end",
  "right-start",
  "right",
  "right-end",
  "bottom-start",
  "bottom",
  "bottom-end",
  "left-start",
  "left",
  "left-end",
];
</script>
