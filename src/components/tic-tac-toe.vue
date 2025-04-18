<script setup lang="ts">
import { ref, reactive } from 'vue';
import circleIcon from '../images/circle.png';
import crossIcon from '../images/cross.png';

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
  <div class="container">
    <h1 class="title">TicTac Game</h1>
    
    <p class="next-player">
      Jogador atual:
       <img
        v-if="currentPlayer === 'X' || currentPlayer === 'O'"
        :src="currentPlayer === 'X' ? crossIcon : circleIcon"
        alt="icon"
        class="icon"
        />
      <p v-if="winner">{{ winner }} wins!</p>
      <p v-else-if="isTie">It's a tie!</p>
    </p>
    
    
      
      <div class="board" v-for="(row, rowIndex) of board" :key="rowIndex">
        <div
        class="squares"
        v-for="(cell, cellIndex) of row"
        :key="cellIndex"
        :class="{ 'cell-x': cell === 'X', 'cell-o': cell === 'O' }"
        :disabled="cell !== null"
        @click="playMove(rowIndex, cellIndex)"
        >
        <img
        v-if="cell === 'X' || cell === 'O'"
        :src="cell === 'X' ? crossIcon : circleIcon"
        alt="icon"
        class="icon"
        />
      </div>
   
    </div>
    <button class="reset" @click="clearTable()">reset game</button>


   
  </div>
</template>

<style>
.container {
 text-align: center;

  height: 100vh;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.title{
      margin-top: 50px;
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
}

.title img{
      padding: 20px 20px;
      height: 50px;
}
.next-player{
      display: flex;
      flex-direction: column;
}
.next-player img{
  padding: 20px 20px;

}

.reset{
  width: 200px;
  height: 80px;
  border: none;

  background-color: brown;
  cursor: pointer;
  border-radius: 40px;
  font-size: 20px;
  margin-top: 80px;
}
.squares{
    display: flex;
    height: 120px;
    width: 120px;
    background:#1f3540;
    border-radius: 12px;
  
}

.squares img{
    height: 80px;
    width: 80px;
    margin: auto;
}

.board{
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      grid-template-rows: repeat(3,1fr);
      width: auto;    /* Largura total do tabuleiro */
    height: 150px;   /* Altura total do tabuleiro */
    margin: 0 auto;  /* Centralizar horizontalmente */
    gap: 10px; /* Espaçamento entre as células */

}
</style>
