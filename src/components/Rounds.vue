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
@keyframes fadeInFromNone {
  0% {
    display: none;
    opacity: 0;
  }

  1% {
    display: block;
    opacity: 0;
  }

  100% {
    display: block;
    opacity: 1;
  }
}
.rounds {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  list-style-type: none;
  gap: 0.5rem;
  padding: 0;
  margin: 1rem;
}
.round {
  height: 50px;
  display: flex;
  flex-direction: column;
  text-align: center;
  border: 2px solid white;
  border-radius: 8px;
  animation: fadeInFromNone 0.5s ease-out;
}
.distance {
  font-size: medium;
  color: lightgrey;
}
.score {
  font-size: large;
  font-weight: bold;
}
</style>
