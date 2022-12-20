<script setup lang="ts">
import Buttons from "./Buttons.vue";
import { ref } from "vue";
import Rounds from "./Rounds.vue";
import Stats from "./Stats.vue";

let successfulPutts = ref<number[]>([]);
let distances = ref<number[]>([10]);
let showStats = ref(false);

function addScore(score: number) {
  successfulPutts.value.push(score);
  distances.value.push(10 - (5 - score));
}

function calculateTotalScore() {
  let totalScore = 0;
  distances.value.forEach((distance, index) => {
    totalScore += distance * successfulPutts.value[index] || 0;
  });
  return totalScore;
}

function undo() {
  if (distances.value.length > 1) {
    successfulPutts.value.splice(successfulPutts.value.length - 1, 1);
    distances.value.splice(distances.value.length - 1, 1);
  }
}

function saveScores() {
  const previousScores = localStorage.getItem("totalScores");
  localStorage.setItem(
    "totalScores",
    previousScores
      ? [previousScores, calculateTotalScore()].toString()
      : [calculateTotalScore()].toString()
  );

  const previousRounds = localStorage.getItem("rounds");
  const round = distances.value.slice(0, 20).map((distance, index) => ({
    distance,
    putts: successfulPutts.value[index],
  }));
  localStorage.setItem(
    "rounds",
    previousRounds
      ? JSON.stringify([...JSON.parse(previousRounds), round])
      : JSON.stringify([round])
  );
  successfulPutts.value.splice(0);
  distances.value.splice(0);
}

function newRound() {
  saveScores();
}
</script>

<template>
  <div class="background"></div>
  <div class="content">
    <h1 class="totalScore">{{ calculateTotalScore() }}</h1>
    <button class="stats-toggle" type="button" @click="showStats = !showStats">
      STATS
    </button>
    <Rounds
      :distances="distances.slice(0, 20)"
      :successfulPutts="successfulPutts"
    />
    <Buttons
      v-if="successfulPutts.length < 20"
      @addScore="(score) => successfulPutts.length < 20 && addScore(score)"
      @undo="undo()"
    />
    <button
      v-if="successfulPutts.length === 20"
      type="button"
      @click="newRound()"
    >
      New round
    </button>
    <Stats v-if="showStats" @click="showStats = !showStats" />
  </div>
</template>

<style scoped>
.background {
  background: url("../assets/background.jpg");
  background-size: cover;
  background-repeat: no-repeat;
  -webkit-filter: blur(8px);
  -moz-filter: blur(8px);
  -o-filter: blur(8px);
  -ms-filter: blur(8px);
  filter: blur(8px);
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}

.content {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}
.totalScore {
  margin-left: 1rem;
  text-shadow: 1px 1px 4px black;
}

.stats-toggle {
  position: absolute;
  right: 1rem;
  top: 1rem;
}
</style>
