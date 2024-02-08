<template>
  <p class="text-3xl">
    {{ label }} <span class="text-slate-400 font-semibold">{{ choice }}</span>
  </p>
</template>

<script setup lang="ts">
defineProps<{
  choice: string | null;
  label: string;
}>();
</script>
