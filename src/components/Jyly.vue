<script setup lang="ts">
import Buttons from "./Buttons.vue";
import { ref } from "vue";
import Rounds from "./Rounds.vue";

const successfulPutts = ref<number[]>([]);
const distances = ref<number[]>([10]);

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
</script>

<template>
  <h1 class="totalScore">{{ calculateTotalScore() }}</h1>
  <Rounds
    :distances="distances.slice(0, 20)"
    :successfulPutts="successfulPutts"
  />
  <Buttons
    @addScore="(score) => successfulPutts.length < 20 && addScore(score)"
  />
</template>

<style scoped>
.totalScore {
  margin-left: 1rem;
}
</style>
