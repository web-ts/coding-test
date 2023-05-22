<script setup lang="ts">
const timeFormat = defineModel<{
  format: "12" | "24";
  ampm: "AM" | "PM";
}>();

function toggleFormat() {
  if (timeFormat.value) {
    timeFormat.value.format = timeFormat.value?.format === "12" ? "24" : "12";
  }
}

</script>

<template>
  <span v-if="timeFormat?.format === '12'" class="ml-2">
    <button
      :class="`${timeFormat.ampm === 'AM' ? 'bg-blue-700' : 'bg-blue-900'} p-2 rounded-l`"
      @click="timeFormat.ampm = 'AM'"
    >
      AM
    </button>
    <button
      :class="`${timeFormat.ampm === 'PM' ? 'bg-blue-700' : 'bg-blue-900'} p-2 rounded-r`"
      @click="timeFormat.ampm = 'PM'"
    >
      PM
    </button>
  </span>
  <button @click="toggleFormat" class="bg-blue-900 p-2 rounded mx-2 shadow-lg">
    Switch to {{ timeFormat?.format === "12" ? "24" : "12" }}h format
  </button>
</template>
