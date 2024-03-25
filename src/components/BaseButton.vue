<template>
  <button
    class="text-lg py-2 px-4 rounded-lg bg-gray-600 transition-colors duration-300 hover:bg-gray-500"
    :class="{ 'hover:bg-red-500': isRed }"
    @click="onClick"
  >
    {{ label }}
  </button>
</template>

<script setup lang="ts">
export interface Props {
  label: string;
  isRed?: boolean;
}

withDefaults(defineProps<Props>(), {
  label: "Click",
  isRed: false,
});

const emit = defineEmits<{
  (e: "click"): void;
}>();

const onClick = () => emit("click");
</script>
