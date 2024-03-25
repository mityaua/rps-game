<template>
  <button>
    <div
      class="bg-white rounded-full shadow-lg w-32 h-32 md:w-48 md:h-48 lg:w-60 lg:h-60 p-7 md:p-10 transition-colors duration-300 hover:bg-stone-400"
      @click="onClick"
    >
      <img :src="imageUrl" :alt="choice" class="w-full h-full" />
      <p class="capitalize text-slate-600 font-semibold">{{ choice }}</p>
    </div>
  </button>
</template>

<script setup lang="ts">
import { computed } from "vue";
import { Action } from "@/enums/Action";
import rockSvg from "@/assets/images/rock.svg";
import paperSvg from "@/assets/images/paper.svg";
import scissorsSvg from "@/assets/images/scissors.svg";

const props = defineProps<{
  choice: string;
}>();

const emit = defineEmits<{
  (e: "choice", value: string): void;
}>();

const imageUrl = computed<string>(() => {
  let url = "";

  switch (props.choice) {
    case Action.Rock:
      url = rockSvg;
      break;
    case Action.Paper:
      url = paperSvg;
      break;
    case Action.Scissors:
      url = scissorsSvg;
      break;
  }

  return url;
});

const onClick = () => {
  emit("choice", props.choice);
};
</script>

<style lang="postcss" scoped>
.drag > div {
  @apply rotate-12;
}

.ghost {
  @apply bg-slate-500 rounded-full;
}

.ghost > div {
  @apply invisible;
}
</style>
