<template>
	<button>
		<div
			class="h-32 w-32 rounded-full bg-white p-7 shadow-lg transition-colors duration-300 hover:bg-stone-400 md:h-48 md:w-48 md:p-10 lg:h-60 lg:w-60"
			@click="onClick"
		>
			<img :src="imageUrl" :alt="choice" class="h-full w-full" />
			<p class="font-semibold capitalize text-slate-600">{{ choice }}</p>
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
	@apply rounded-full bg-slate-500;
}

.ghost > div {
	@apply invisible;
}
</style>
