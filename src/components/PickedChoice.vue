<template>
	<p class="text-3xl">
		{{ label }} <span class="font-semibold text-slate-400">{{ choice }}</span>
	</p>
</template>

<script setup lang="ts">
defineProps<{
	choice: string | null;
	label: string;
}>();
</script>
