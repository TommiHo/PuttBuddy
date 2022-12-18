<script setup lang="ts">
const emit = defineEmits(["addScore", "undo"]);

function addScore(score: number) {
  emit("addScore", score);
}
</script>

<template>
  <div class="buttons-container">
    <button class="button undo" type="button" @click="emit('undo')">
      UNDO
    </button>
    <div class="buttons">
      <button class="button" type="button" @click="addScore(0)">0</button>
      <button class="button" type="button" @click="addScore(1)">1</button>
      <button class="button" type="button" @click="addScore(2)">2</button>
      <button class="button" type="button" @click="addScore(3)">3</button>
      <button class="button" type="button" @click="addScore(4)">4</button>
      <button class="button" type="button" @click="addScore(5)">5</button>
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
.button {
  background-color: transparent;
  border: 1px solid white;
  height: 4rem;
  font-weight: bold;
  font-size: medium;
}
.undo {
  align-self: flex-end;
  border-radius: 8px;
  height: 2rem;
}
</style>
