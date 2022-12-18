<script setup lang="ts">
const roundsPlayed = localStorage.getItem("rounds");
const allScores = localStorage
  .getItem("totalScores")
  ?.split(",")
  .map((score) => +score);
</script>

<template>
  <div class="modal">
    <h2>Stats</h2>
    <h3 v-if="roundsPlayed">
      Rounds played: {{ JSON.parse(roundsPlayed).length }}
    </h3>
    <h3 v-if="allScores">Best round: {{ Math.max(...allScores) }}</h3>
  </div>
</template>

<style scoped>
.modal {
  z-index: 1;
  background-color: green;
  opacity: 0.9;
  position: absolute;
  top: 1rem;
  bottom: 1rem;
  left: 1rem;
  right: 1rem;
  padding: 0 1rem;
}
</style>
