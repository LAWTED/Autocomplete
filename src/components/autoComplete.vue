<template>
  <div ref="dropdownRef" class="container">
    <input v-model="text" type="text" @focusin="setFilterItems" />
    <transition-group name="list" tag="ul">
      <li
        v-for="(item, index) in filterItems"
        :key="item"
        :class="{ active: index == selected }"
        @click="setText(item)"
      >
        {{ item }}
      </li>
    </transition-group>
  </div>
</template>
<script setup lang="ts">
import { ref, watch } from "vue";
import useClickOutside from "./useClickOutside";

const props = defineProps<{
  value: string;
  items: string[];
}>();
const filterItems = ref<string[]>([]);
const text = ref("");
const selected = ref(-1);
const setFilterItems = () => {
  filterItems.value = props.items;
};
const setText = (item: string) => {
  text.value = item;
};
watch(text, (text) => {
  if (text) {
    filterItems.value = props.items.filter((item) =>
      item.toLowerCase().startsWith(text.toLowerCase())
    );
  } else {
    filterItems.value = props.items;
  }
});

const dropdownRef = ref<null | HTMLElement>(null);
const isClickOutside = useClickOutside(dropdownRef);
watch(isClickOutside, () => {
  if (isClickOutside.value) {
    filterItems.value = [];
  }
});
</script>

<style scoped>
.container {
  width: fit-content;
}
input {
  border-radius: 10px;
  padding: 5px;
  border: 1px solid #ccc;
}
ul {
  list-style: none;
  padding: 0;
  margin: 0;
  border-radius: 5px;
  margin-top: 10px;
}
li {
  cursor: pointer;
  padding: 3px;
  margin: 5px;
  background: #eee;
  border-radius: 5px;
  transition: all 0.3s;
}
li:hover {
  background: #ccc;
  scale: 1.02;
}
.list-move, /* apply transition to moving elements */
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

/* ensure leaving items are taken out of layout flow so that moving
   animations can be calculated correctly. */
.list-leave-active {
  position: absolute;
}
</style>
