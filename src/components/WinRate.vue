<template>
	<p class="text-lg">Win rate: {{ roundRate }}%</p>
</template>

<script setup lang="ts">
import { computed } from "vue";

const props = defineProps<{
	rate: number;
}>();

const roundRate = computed<number>(() => Math.round(props.rate));
</script>
