<template>
  <div>
    <input v-model="text" type="text" />
    <ul>
      <li v-for="item in filterItems" :key="item">
        {{ item }}
      </li>
    </ul>
  </div>
</template>
<script setup lang="ts">
import { ref, watch } from "vue";

const props = defineProps<{
  value: string;
  items: string[];
}>();
const filterItems = ref<string[]>([]);
const text = ref("");
watch(text, (text) => {
  if (text) {
    filterItems.value = props.items.filter((item) =>
      item.toLowerCase().startsWith(text.toLowerCase())
    );
  } else {
    filterItems.value = [];
  }
});
</script>

<style scoped></style>
