<template>
  <button
    class="bg-white rounded-full shadow-lg w-32 md:w-48 lg:w-60 p-4 md:p-10 transition-colors duration-300 hover:bg-stone-400"
    @click="onClick"
  >
    <img :src="imageUrl" :alt="choice" class="w-full h-full" />
  </button>
</template>

<script setup lang="ts">
import { computed } from "vue";
import rockSvg from "../assets/images/rock.svg";
import paperSvg from "../assets/images/paper.svg";
import scissorsSvg from "../assets/images/scissors.svg";

const props = defineProps<{
  choice: string;
}>();

const emit = defineEmits<{
  (e: "choice", value: string): void;
}>();

const imageUrl = computed<string>(() => {
  let url = "";

  switch (props.choice) {
    case "rock":
      url = rockSvg;
      break;
    case "paper":
      url = paperSvg;
      break;
    case "scissors":
      url = scissorsSvg;
      break;
  }

  return url;
});

const onClick = () => {
  emit("choice", props.choice);
};
</script>
