<script setup lang="ts">
import { computed } from "vue";

// This will need to take a prop that supports two way binidng. In vue we call that a v-model
const model = defineModel<number>();

// We need to list a range of times eg minutes or hours and this needs to be dynamic
const props = defineProps<{
  rangeStart: number;
  rangeEnd: number;
}>();

const items = computed(() => {
  const items: { value: number; label: string }[] = [];

  for (let i = props.rangeStart; i <= props.rangeEnd; i++) {
    items.push({
      value: i,
      label: i.toString().padStart(2, "0")
    });
  }

  return items;
});
</script>

<template>
  <select v-model="model" class="p-2 bg-blue-900 rounded shadow-lg focus:outline-none focus:ring">
    <option v-for="item in items" :key="item.value" :value="item.value">{{ item.label }}</option>
  </select>
</template>

<style scoped>
select {
  appearance: none;
}
/* Let's make the scroll bar a bit better looking as well */
select::-webkit-scrollbar {
  width: 4px;
}
select::-webkit-scrollbar-track {
  border-radius: 4px;
}
select::-webkit-scrollbar-thumb {
  border-radius: 4px;
  background-color: rgba(0, 0, 0, 0.2);
}
</style>
