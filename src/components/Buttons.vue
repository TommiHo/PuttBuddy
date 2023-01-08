<script setup lang="ts">
const emit = defineEmits(["addScore", "undo"]);

function addScore(score: number) {
  emit("addScore", score);
}
</script>

<template>
  <div class="buttons-container">
    <button class="undo" type="button" @click="emit('undo')">UNDO</button>
    <div class="buttons">
      <button class="scoreButton" type="button" @click="addScore(0)">0</button>
      <button class="scoreButton" type="button" @click="addScore(1)">1</button>
      <button class="scoreButton" type="button" @click="addScore(2)">2</button>
      <button class="scoreButton" type="button" @click="addScore(3)">3</button>
      <button class="scoreButton" type="button" @click="addScore(4)">4</button>
      <button class="scoreButton" type="button" @click="addScore(5)">5</button>
    </div>
  </div>
</template>

<style scoped>
.buttons-container {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  margin: 1rem;
  display: flex;
  flex-direction: column;
  gap: 2rem;
}
.buttons {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
}

.scoreButton {
  height: 6rem;
  font-size: x-large;
  border: 0;
  border-radius: 0;
}
.scoreButton:not(:first-child) {
  border-left: 1px solid white;
}
.undo {
  align-self: flex-end;
}
</style>
