<script setup lang="ts">
const emit = defineEmits(["addScore"]);

function addScore(score: number) {
  emit("addScore", score);
}
</script>

<template>
  <div class="buttons">
    <button class="button" type="button" @click="addScore(0)">0</button>
    <button class="button" type="button" @click="addScore(1)">1</button>
    <button class="button" type="button" @click="addScore(2)">2</button>
    <button class="button" type="button" @click="addScore(3)">3</button>
    <button class="button" type="button" @click="addScore(4)">4</button>
    <button class="button" type="button" @click="addScore(5)">5</button>
  </div>
</template>

<style scoped>
.buttons {
  position: absolute;
  bottom: 2rem;
  left: 1rem;
  right: 1rem;
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: 0.1rem;
}

.button {
  background-color: transparent;
  border: 1px solid white;
  height: 2rem;
  font-weight: bold;
}
</style>
