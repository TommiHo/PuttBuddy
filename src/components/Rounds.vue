<script setup lang="ts">
defineProps({
  distances: {
    type: Array<Number>,
    default: [10],
  },
  successfulPutts: {
    type: Array<Number>,
    default: [],
  },
});
</script>

<template>
  <ol class="rounds">
    <li class="round" v-for="(distance, index) in distances">
      <span class="distance">{{ distance }}m</span>
      <span class="score">{{ successfulPutts[index] }}</span>
    </li>
  </ol>
</template>

<style scoped>
.rounds {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  list-style-type: none;
  gap: 1rem;
  padding: 0;
  margin: 1rem;
}
.round {
  height: 50px;
  display: flex;
  flex-direction: column;
  text-align: center;
  padding: 0.5rem;
  border: 1px solid white;
}
.distance {
  font-size: 12px;
}
.score {
  font-size: 18px;
  font-weight: bold;
}
</style>
