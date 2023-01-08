<script setup lang="ts">
import { ref } from "vue";
type Score = {
  rounds: { distance: number; putts: number }[];
  score: number;
  date: Date;
};

const emit = defineEmits(["close"]);

const scores = localStorage.getItem("scores");
const allScores: Score[] = scores && JSON.parse(scores);
const totalScores = allScores && allScores.map((i) => i.score);
const average =
  totalScores && totalScores.reduce((a, b) => a + b, 0) / totalScores.length;

let clearClickCount = ref(0);

function clearHistory() {
  clearClickCount.value++;
  if (clearClickCount.value > 1) localStorage.clear();
}
</script>

<template>
  <div class="modal">
    <h1 class="heading">STATS</h1>
    <button class="close-button" type="button" @click="emit('close')">
      <img src="../assets/icon_close.svg" />
    </button>
    <div v-if="clearClickCount <= 1" class="content">
      <div class="row" v-if="scores">
        <h3 class="title">Games played:</h3>
        <h2 class="score">{{ JSON.parse(scores).length }}</h2>
      </div>
      <div class="row" v-if="totalScores">
        <h3 class="title">Best:</h3>
        <h2 class="score">{{ Math.max(...totalScores) }}</h2>
      </div>
      <div class="row" v-if="average">
        <h3 class="title">Average:</h3>
        <h2 class="score">{{ Math.round(average) }}</h2>
      </div>
      <div v-if="totalScores" class="previous-scores">
        <h3>10 previous games:</h3>
        <div v-for="score in allScores">
          <span>{{ new Date(score.date).toLocaleDateString("fi-FI") }}: </span>
          <b>{{ score.score }}</b>
        </div>
      </div>
    </div>
    <h3 v-if="!scores">Couldn't find any history data for this browser.</h3>
    <div class="clear-button-container">
      <button
        class="clear-button"
        v-if="scores && clearClickCount < 2"
        type="button"
        @click="clearHistory()"
      >
        Clear history
      </button>
      <p class="confirmation-message" v-if="clearClickCount === 1">
        Click again to confirm.
      </p>
    </div>
  </div>
</template>

<style scoped>
.heading {
  text-align: center;
}
.modal {
  z-index: 1;
  background-color: black;
  position: absolute;
  top: 1rem;
  bottom: 1rem;
  left: 1rem;
  right: 1rem;
  padding: 1rem;
  overflow: auto;
}

.close-button {
  position: absolute;
  right: 0.5rem;
  top: 0.5rem;
  font-size: x-large;
  border: 0;
}

.row {
  display: flex;
  justify-content: space-between;
}

.previous-scores {
  list-style-type: none;
}

.clear-button-container {
  margin-top: 3rem;
  position: relative;
  display: flex;
  gap: 1rem;
}

.clear-button {
  height: 50px;
}

.confirmation-message {
  width: 50%;
  margin: 0;
}
</style>
