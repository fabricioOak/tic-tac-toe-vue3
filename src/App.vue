<script setup lang="ts">
import { ref } from "vue";

let currentTurn = ref<string>("X");
let square = ref<string[]>([]);
let gameOver = ref<boolean>(false);

for (let i = 0; i <= 9; i++) {
  square.value.push("empty");
}
square.value[0] = "nothing";

const clickedSquare = (n: number) => {
  if (square.value[n] !== "empty") return;
  square.value[n] = currentTurn.value;
  isWinner();
  isCat();
  currentTurn.value === "X"
    ? (currentTurn.value = "O")
    : (currentTurn.value = "X");
  console.log(currentTurn.value);
};

const isWinner = () => {
  const lines = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
    [1, 4, 7],
    [2, 5, 8],
    [3, 6, 9],
    [1, 5, 9],
    [3, 5, 7],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (
      square.value[a] !== "empty" &&
      square.value[a] === square.value[b] &&
      square.value[a] === square.value[c]
    ) {
      gameOver.value = true;
    }
  }
};

const isCat = () => {
  let isCats = true;
  square.value.forEach((item) => {
    if (item === "empty") {
      isCats = false;
    }
  });
  if (isCats === true) {
    gameOver.value = true;
  }
};
</script>

<template>
  <div v-if="!gameOver" id="board">
    <div class="square" v-for="n in 9" :key="n" @click="clickedSquare(n)">
      {{ square[n] }}
    </div>
  </div>
</template>

<style>
#board {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  width: 400px;
  height: 400px;
  margin: 0 auto;
}
.square {
  width: 30%;
  border: 2px solid #000;
  color: #000;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
