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
  console.log(distances, successfulPutts);
  if (distances.value.length > 1) {
    successfulPutts.value.splice(successfulPutts.value.length - 1, 1);
    distances.value.splice(distances.value.length - 1, 1);
  }
}

function saveScores() {
  const previousScores = localStorage.getItem("scores");
  const roundData = {
    rounds: distances.value.slice(0, 20).map((distance, index) => ({
      distance,
      putts: successfulPutts.value[index],
    })),
    score: calculateTotalScore(),
    date: new Date(),
  };
  localStorage.setItem(
    "scores",
    previousScores
      ? JSON.stringify([...JSON.parse(previousScores), roundData])
      : JSON.stringify([roundData])
  );
  successfulPutts.value.splice(0);
  distances.value.splice(1);
}

function startNewGame() {
  saveScores();
}
</script>

<template>
  <div class="background"></div>
  <div class="content">
    <div class="score-row">
      <h1 class="totalScore">{{ calculateTotalScore() }}</h1>
      <div class="next-distance">
        <span>Next distance:</span>
        <span class="distance">{{ distances[distances.length - 1] }}m</span>
      </div>
    </div>
    <button class="stats-toggle" type="button" @click="showStats = !showStats">
      STATS
    </button>
    <Rounds
      :distances="distances.slice(0, 20)"
      :successfulPutts="successfulPutts"
    />
    <Buttons
      @addScore="(score) => successfulPutts.length < 20 && addScore(score)"
      @undo="undo()"
    />
    <div class="new-game-button-container">
      <button
        v-if="successfulPutts.length === 20"
        type="button"
        class="new-game-button"
        @click="startNewGame()"
      >
        New game
      </button>
    </div>
    <Stats v-if="showStats" @close="showStats = !showStats" />
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

.score-row {
  display: flex;
}
.totalScore {
  margin-left: 1rem;
  text-shadow: 1px 1px 4px black;
  width: 90px;
}

.next-distance {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.distance {
  font-size: larger;
  font-weight: bold;
  text-shadow: 1px 1px 4px black;
}

.new-game-button-container {
  width: 100%;
  display: flex;
  justify-content: center;
  position: absolute;
  bottom: 30%;
}

.new-game-button {
  font-size: x-large;
  padding: 1rem;
  width: unset;
  height: unset;
}

.stats-toggle {
  position: absolute;
  right: 1rem;
  top: 1rem;
}
</style>
