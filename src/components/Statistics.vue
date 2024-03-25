<template>
  <p class="text-2xl">
    <span class="text-green-300">Wins ({{ wins }})</span> : <span class="text-red-300">Losses ({{ losses }})</span> :
    <span class="text-yellow-300">Draws ({{ draws }})</span>
  </p>
</template>

<script setup lang="ts">
defineProps<{
  draws: number;
  losses: number;
  wins: number;
}>();
</script>
