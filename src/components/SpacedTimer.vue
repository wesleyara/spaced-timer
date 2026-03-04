<script setup lang="ts">
import { computed, ref } from "vue";

const times = ref(5);
const seconds = ref(5);

const displayTime = ref(0);
const countTimes = ref(0);
const cycle = ref(0);
const interval = ref<null | number>(null);
const isEvenCount = computed(() => countTimes.value % 2 === 0);

const buttonText = computed(() => {
  if (interval.value) return "Stop";

  return "Start";
});

const buttonAction = computed(() => {
  if (interval.value) return onStop;

  return onStart;
});

function onStop() {
  if (interval.value) {
    clearInterval(interval.value);
    interval.value = null;
    displayTime.value = 0;
    countTimes.value = 0;
    cycle.value = 0;
  }
}

function onStart() {
  if (interval.value) {
    clearInterval(interval.value);
  }

  const intervalId = setInterval(() => {
    if (cycle.value >= times.value) {
      return onStop();
    }

    if (displayTime.value === seconds.value) {
      displayTime.value = 0;
      countTimes.value += 1;

      if (isEvenCount.value) {
        cycle.value += 1;
      }
    } else {
      displayTime.value += 1;
    }
  }, 1000);

  interval.value = intervalId;
}
</script>

<template>
  <div class="flex w-full flex-col items-center gap-4">
    <span class="flex w-full flex-col gap-2">
      <span class="flex w-full flex-col">
        <label for="times">Times</label>
        <input id="times" placeholder="Times" v-model="times" type="number" />
      </span>
      <span class="flex w-full flex-col">
        <label for="seconds">Seconds</label>
        <input id="seconds" placeholder="Seconds" v-model="seconds" type="number" />
      </span>
      <button @click="buttonAction">{{ buttonText }}</button>
    </span>

    <span
      :class="[
        'flex size-40 items-center justify-center rounded-full text-2xl',
        {
          'bg-green-300': isEvenCount,
          'bg-red-300': !isEvenCount,
        },
      ]"
    >
      {{ displayTime }}
    </span>

    <span>Count times: {{ cycle + 1 }}</span>
  </div>
</template>
