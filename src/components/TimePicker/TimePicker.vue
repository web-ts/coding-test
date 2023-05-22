<script setup lang="ts">
import { computed, reactive, ref, watch } from "vue";
import TimePickerList from "./TimePickerList.vue";
import TimePickerFormatChanger from "./TimePickerFormatChanger.vue";

const time = defineModel<{
  hours: number;
  minutes: number;
}>({
  required: true
});

const timeFormat = reactive({
  format: "24" as "12" | "24",
  ampm: "AM" as "AM" | "PM" // the default for 12 hour format
});

const hoursRange = computed(() => {
  if (timeFormat.format === "12") {
    return {
      rangeStart: 1,
      rangeEnd: 12
    };
  } else {
    return {
      rangeStart: 0,
      rangeEnd: 23
    };
  }
});

// Now that the hours range is dynamic we need to make sure that our hours are within that range and update them if they are not
// We'll use a watcher for that. Let's actually watch for the time format changeing

watch(timeFormat, () => {
  // Now let's check if we are in a 12 hour format and if the hours are not within the range
  if (timeFormat.format === "12") {
    if (time.value.hours > 12) {
      time.value.hours = time.value.hours - 12;
      // This should also set the time to PM
      timeFormat.ampm = "PM";
    }
    if (time.value.hours === 0) {
      time.value.hours = 12;
      // And this one to AM
      timeFormat.ampm = "AM";
    }
  }

  // Also we if we change to a 24 hour format we need to check if the time is in AM or PM
  if (timeFormat.format === "24" && timeFormat.ampm === "AM" && time.value.hours === 12) {
    time.value.hours = 0;
  }
});

// Let's make a computed variable of the time so we can print it in the locale string

const localeTime = computed(() => {
  // We assume our Time is in UTC so we need to convert it to the local time

  let hours = time.value.hours;

  // We need to the same logic as the one used for switching the format
  if (timeFormat.format === "12" && timeFormat.ampm === "AM" && hours === 12) {
    hours = 0;
  }

  const utcTime = new Date(Date.UTC(0, 0, 0, hours, time.value.minutes));

  // Now we can return the local time
  return utcTime.toLocaleTimeString();
});

const mode = ref<"edit" | "saved">("edit");
</script>

<template>
  <div v-if="mode === 'edit'">
    <TimePickerList v-model="time.hours" :range-start="hoursRange.rangeStart" :range-end="hoursRange.rangeEnd" />
    <span> : </span>
    <TimePickerList v-model="time.minutes" :range-start="0" :range-end="59" />
    <TimePickerFormatChanger v-model="timeFormat" />
    <button class="bg-green-600 p-2 rounded shadow-lg" @click="mode = 'saved'">Save</button>
  </div>
  <!-- This would be the time display -->
  <div v-else class="flex justify-center items-center">
    <p>You have selected:</p>
    <span class="bg-gray-700 p-2 rounded mx-2"> {{ localeTime }} </span>
    <button class="bg-blue-600 p-2 rounded shadow-lg" @click="mode = 'edit'">Edit</button>
  </div>
</template>
