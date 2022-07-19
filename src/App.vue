<script setup lang="ts">
import { ref, Ref, computed, ComputedRef, onMounted } from "vue";
import { Outcomes, OutcomesItem } from "./interfaces/Outcomes";

// Stats values
const wins: Ref<number> = ref(0);
const losses: Ref<number> = ref(0);
const draws: Ref<number> = ref(0);

const userChoice: Ref<string | null> = ref(null);
const computerChoice: Ref<string | null> = ref(null);

const verdict: Ref<string | null> = ref(null);

const outcomes: Outcomes = {
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
const winsPercentage: ComputedRef<number> = computed(() => {
  const total = wins.value + losses.value + draws.value;
  return total ? (wins.value / total) * 100 : 0;
});

// Function on button click
const play = (choice: string): void => {
  userChoice.value = choice;

  const choises = ["rock", "paper", "scissors"];
  const random = Math.floor(Math.random() * choises.length);
  computerChoice.value = choises[random];

  const outcome = outcomes[choice as keyof Outcomes][computerChoice.value as keyof OutcomesItem];

  if (outcome === "win") {
    wins.value++;
    verdict.value = "You win!";
  } else if (outcome === "lose") {
    losses.value++;
    verdict.value = "You lose!";
  } else {
    draws.value++;
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

onMounted(() => {
  loadGame();

  window.addEventListener("keypress", (event: KeyboardEvent) => {
    if (event.key === "r") {
      resetRound();
    }
  });
});
</script>

<template>
  <div class="bg-gradient-to-b from-gray-900 to-gray-600 bg-gradient-to-r text-white text-center min-h-screen flex flex-col">
    <!-- Header -->
    <header class="container mx-auto p-6">
      <h1 class="text-4xl font-bold">Rock, Paper, Scissors</h1>
    </header>

    <main class="container mx-auto p-6 flex-1">
      <!-- Start screen with buttons -->
      <Transition name="slide-up" mode="out-in">
        <div v-if="userChoice === null" class="flex items-center justify-center flex-wrap gap-4 md:gap-8">
          <button
            @click="play('rock')"
            class="bg-white rounded-full shadow-lg w-32 md:w-48 lg:w-64 p-4 md:p-10 transition-colors duration-300 hover:bg-stone-400"
          >
            <img src="./assets/images/rock.svg" alt="Rock" class="w-full h-full" />
          </button>

          <button
            @click="play('paper')"
            class="bg-white rounded-full shadow-lg w-32 md:w-48 lg:w-64 p-4 md:p-10 transition-colors duration-300 hover:bg-cyan-200"
          >
            <img src="./assets/images/paper.svg" alt="Paper" class="w-full h-full" />
          </button>

          <button
            @click="play('scissors')"
            class="bg-white rounded-full shadow-lg w-32 md:w-48 lg:w-64 p-4 md:p-10 transition-colors duration-300 hover:bg-lime-300"
          >
            <img src="./assets/images/scissors.svg" alt="Scissors" class="w-full h-full" />
          </button>
        </div>

        <!-- Results -->
        <div v-else>
          <div class="text-3xl mb-4">
            <p>
              You picked <span class="text-slate-400 font-semibold">{{ userChoice }}</span>
            </p>
          </div>

          <div class="text-3xl mb-4">
            <p>
              Computer picked <span class="text-slate-400 font-semibold">{{ computerChoice }}</span>
            </p>
          </div>

          <!-- Verdict block -->
          <div
            class="text-6xl mb-12"
            :class="{
              'text-green-300': verdict === 'You win!',
              'text-red-300': verdict === 'You lose!',
              'text-yellow-300': verdict === 'Draw!',
            }"
          >
            {{ verdict }}
          </div>

          <!-- Again button -->
          <button
            @click="resetRound"
            title="Or R on keyboard"
            class="bg-gray-600 text-lg py-2 px-4 rounded-lg transition-colors duration-300 hover:bg-gray-500"
          >
            Again
          </button>
        </div>
      </Transition>

      <!-- Statistics -->
      <Transition name="slide-up" mode="out-in">
        <div v-if="wins || losses || draws">
          <div class="mt-12 mb-4">
            <p class="text-2xl">
              <span class="text-green-300">Wins ({{ wins }})</span> : <span class="text-red-300">Losses ({{ losses }})</span> :
              <span class="text-yellow-300">Draws ({{ draws }})</span>
            </p>
          </div>

          <!-- Win rate block -->
          <div>
            <p class="text-lg">Win rate: {{ Math.round(winsPercentage) }}%</p>
          </div>

          <!-- Clear statistics button -->
          <div class="mt-6 mb-6">
            <button
              @click="clearStatistics"
              class="bg-gray-600 hover:bg-red-500 opacity-70 hover:opacity-100 text-lg py-2 px-4 rounded-lg transition-colors duration-300"
            >
              Clear statistics
            </button>
          </div>
        </div>
      </Transition>
    </main>
  </div>
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
