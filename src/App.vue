<script setup lang="ts">
import { reactive } from "vue";
import TimePicker from "./components/TimePicker/TimePicker.vue";

// Reactive time variable; similar to createSignal in React/Solid
const time = reactive({
  hours: 0,
  minutes: 0
});

</script>

<template>
  <!-- Let's center everything -->
  <div class="flex flex-col justify-center items-center min-h-screen">
    <TimePicker v-model="time" />
  </div>
</template>
