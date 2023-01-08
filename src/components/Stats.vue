<script setup lang="ts">
type Score = {
  rounds: { distance: number; putts: number }[];
  score: number;
  date: Date;
};
const scores = localStorage.getItem("scores");
const allScores: Score[] = scores && JSON.parse(scores);
const totalScores = allScores && allScores.map((i) => i.score);
const average =
  totalScores && totalScores.reduce((a, b) => a + b, 0) / totalScores.length;
</script>

<template>
  <div class="modal">
    <h1 class="heading">STATS</h1>
    <button class="close-button" type="button">
      <img src="../assets/icon_close.svg" />
    </button>
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
    <h3 v-if="!scores">You haven't played any games yet</h3>
  </div>
</template>

<style scoped>
.heading {
  text-align: center;
}
.modal {
  z-index: 1;
  background-color: black;
  opacity: 0.8;
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
</style>
