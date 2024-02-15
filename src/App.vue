<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from "vue";
import Choice from "./interfaces/Choice";
import ChoicesVariants from "./interfaces/ChoicesVariants";
import Page from "./components/Page.vue";
import Container from "./components/Container.vue";
import Header from "./components/Header.vue";
import ChoiceButton from "./components/ChoiceButton.vue";
import PickedChoice from "./components/PickedChoice.vue";
import Verdict from "./components/Verdict.vue";
import Statistics from "./components/Statistics.vue";
import WinRate from "./components/WinRate.vue";
import BaseButton from "./components/BaseButton.vue";

// Stats values
const wins = ref<number>(0);
const losses = ref<number>(0);
const draws = ref<number>(0);

const userChoice = ref<string | null>(null);
const computerChoice = ref<string | null>(null);

const verdict = ref<string | null>(null);

const outcomes: ChoicesVariants = {
  rock: {
    rock: "draw",
    paper: "lose",
    scissors: "win",
  },
  paper: {
    rock: "win",
    paper: "draw",
    scissors: "lose",
  },
  scissors: {
    rock: "lose",
    paper: "win",
    scissors: "draw",
  },
};

// Wins statistics
const winsPercentage = computed<number>(() => {
  const total = wins.value + losses.value + draws.value;
  return total ? (wins.value / total) * 100 : 0;
});

// Function on button click
const play = (choice: string): void => {
  userChoice.value = choice;

  const choicesArray = ["rock", "paper", "scissors"];
  const random = Math.floor(Math.random() * choicesArray.length);
  computerChoice.value = choicesArray[random];

  const choiceKey = choice as keyof Choice;
  const computerChoiceKey = computerChoice.value as keyof ChoicesVariants;

  const outcome = outcomes[choiceKey][computerChoiceKey];

  if (outcome === "win") {
    wins.value += 1;
    verdict.value = "You win!";
  } else if (outcome === "lose") {
    losses.value += 1;
    verdict.value = "You lose!";
  } else {
    draws.value += 1;
    verdict.value = "Draw!";
  }

  saveGame();
};

// Save game to local storage
const saveGame = (): void => {
  localStorage.setItem("wins", wins.value.toString());
  localStorage.setItem("losses", losses.value.toString());
  localStorage.setItem("draws", draws.value.toString());
};

// Get game stats from local storage
const loadGame = (): void => {
  wins.value = Number(localStorage.getItem("wins") || 0);
  losses.value = Number(localStorage.getItem("losses") || 0);
  draws.value = Number(localStorage.getItem("draws") || 0);
};

// Reset game stats
const resetRound = (): void => {
  userChoice.value = null;
  computerChoice.value = null;
  verdict.value = null;
};

const clearStatistics = (): void => {
  wins.value = 0;
  losses.value = 0;
  draws.value = 0;

  localStorage.removeItem("wins");
  localStorage.removeItem("losses");
  localStorage.removeItem("draws");

  resetRound();
};

const keypressHandler = (event: KeyboardEvent) => {
  if (event.key === "r") {
    resetRound();
  }
};

onMounted(() => {
  loadGame();

  window.addEventListener("keypress", keypressHandler);
});

onUnmounted(() => {
  window.removeEventListener("keypress", keypressHandler);
});
</script>

<template>
  <Page>
    <Header />

    <Container>
      <!-- Start screen with buttons -->
      <Transition name="slide-up" mode="out-in">
        <div v-if="!userChoice" class="flex items-center justify-center flex-wrap gap-4 md:gap-8">
          <ChoiceButton choice="rock" @choice="play" />

          <ChoiceButton choice="paper" @choice="play" />

          <ChoiceButton choice="scissors" @choice="play" />
        </div>

        <!-- Results -->
        <div v-else>
          <div class="mb-8">
            <PickedChoice label="You picked" :choice="userChoice" />
            <PickedChoice label="Computer picked" :choice="computerChoice" />
          </div>

          <Verdict class="mb-8" :verdict="verdict" />

          <BaseButton label="Again" title="Or R on keyboard" @click="resetRound" />
        </div>
      </Transition>

      <!-- Statistics -->
      <Transition name="slide-up" mode="out-in">
        <div v-if="wins || losses || draws">
          <Statistics class="mt-8 mb-4" :draws="draws" :losses="losses" :wins="wins" />

          <!-- Win rate block -->
          <WinRate :rate="winsPercentage" />

          <!-- Clear statistics button -->
          <BaseButton isRed class="mt-6 mb-6" label="Clear statistics" @click="clearStatistics" />
        </div>
      </Transition>
    </Container>
  </Page>
</template>

<style>
.slide-up-enter-active,
.slide-up-leave-active {
  transition: all 0.15s ease-out;
}

.slide-up-enter-from {
  opacity: 0;
  transform: translateY(30px);
}

.slide-up-leave-to {
  opacity: 0;
  transform: translateY(-30px);
}
</style>
