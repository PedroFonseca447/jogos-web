<script setup lang="ts">
import { ref, reactive } from 'vue';

const winner = ref<string | null>(null);
const isTie = ref(false);
const finalGame = ref(false);
const currentPlayer = ref('X');

let board = reactive([
  ['', '', ''],
  ['', '', ''],
  ['', '', ''],
]);

const checkTie = () => {
  for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
      if (!board[i][j]) {
        return false;
      }
    }
  }
  return true;
};

const checkWin = () => {
  const a = currentPlayer.value;
  for (let i = 0; i < 3; i++) {
    //linhas
    if (board[i].every((cell) => cell === a)) return true;
    //colunas
    if (board.every((row) => row[i] === a)) return true;
    //valida nas diagonais
    if (board[0][0] === a && board[1][1] === a && board[2][2] === a) return true;
    if (board[0][2] === a && board[1][1] === a && board[2][0] === a) return true;
  }
  return false;
};

const playMove = (row: number, col: number) => {
  if (!board[row][col] && !winner.value) {
    board[row][col] = currentPlayer.value;

    if (checkWin()) {
      winner.value = currentPlayer.value;
    } else if (checkTie()) {
      isTie.value = true;
    } else {
      currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';
    }
  }
};

const clearTable = () => {
  board = [
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],
  ];

  currentPlayer.value = 'X';
  finalGame.value = false;
  isTie.value = false;
  winner.value = null;
};
</script>

<template>
  <div class="component">
    <h1 class="">TicTac Game</h1>

    <p class="">
      Jogador atual: <span class="">{{ currentPlayer }}</span>
    </p>
    <div class="row" v-for="(row, rowIndex) of board" :key="rowIndex">
      <div
        class="cell"
        v-for="(cell, cellIndex) of row"
        :key="cellIndex"
        :class="{ 'cell-x': cell === 'X', 'cell-o': cell === 'O' }"
        :disabled="cell !== null"
        @click="playMove(rowIndex, cellIndex)"
      >
        {{ cell }}
      </div>
    </div>

    <div class="buttonAndResult">
      <p v-if="winner">{{ winner }} wins!</p>
      <p v-else-if="isTie">It's a tie!</p>
      <button @click="clearTable()">reset game</button>
    </div>
  </div>
</template>

<style>
body {
  margin: 0;
  display: flex;
  min-width: 320px;
  min-height: 100vh;
}

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
}
.tic-tac-toe {
  text-align: center;
}

.board {
  display: inline-flex;
  margin-top: 50px;
  background-color: #f00;
}

.row {
  clear: both;
}

.cell {
  width: 50px;
  height: 50px;
  float: left;
  margin-right: -1px;
  margin-bottom: -1px;
  line-height: 50px;
  text-align: center;
  border: 1px solid #bbb;
  cursor: pointer;
  font-size: 40px;
}

.cell-x {
  color: #f00;
}

.cell-o {
  color: #00f;
}

button {
  margin-top: 20px;
  font-size: 16px;
  padding: 10px;
  border-radius: 5px;
  background-color: #ccc;
  border: none;
  cursor: pointer;
}
</style>
