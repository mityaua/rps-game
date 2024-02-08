<template>
  <p class="text-6xl" :class="verdictColor">
    {{ verdict }}
  </p>
</template>

<script setup lang="ts">
import { computed } from "vue";

const props = defineProps<{
  verdict: string | null;
}>();

const verdictColor = computed<string>(() => {
  let color = "";

  switch (props.verdict) {
    case "Draw!":
      color = "text-yellow-300";
      break;
    case "You lose!":
      color = "text-red-300";
      break;
    case "You win!":
      color = "text-green-300";
      break;
  }

  return color;
});
</script>
