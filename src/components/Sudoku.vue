<template>
  <div class="sudoku">
    <h1>Sudoku - Dificuldade: {{ dificultSelect }}</h1>

    <div v-for="(row, i) in board" :key="i" class="row">
      <div
        v-for="(cell, j) in row"
        :key="j"
        class="cell"
        :class="{ fixed: originalBoard[i][j] !== 0, selected: selectRow === i && selectCol === j }"
        @click="selectCell(i, j)"
      >
        {{ cell !== 0 ? cell : '' }}
      </div>
    </div>

    <div class="numbers">
      <button
        v-for="n in 9"
        :key="n"
        class="changed"
        @click="setValue(n)"
        @keydown.delete="deleteValue()"
      >
        {{ n }}
      </button>
    </div>

    <div class="controls">
      <button @click="resetGame(selectDificult())">Novo Jogo</button>
      <button @click="checkVictory">Validar</button>
    </div>

    <div class="dificuldade">
      <button @click="changeDifficulty('facil')">Fácil</button>
      <button @click="changeDifficulty('medio')">Médio</button>
      <button @click="changeDifficulty('dificil')">Difícil</button>
    </div>

    <div v-if="victory === true" class="message success">Parabéns! Sudoku completo 🎉</div>
    <div v-else-if="victory === false" class="message error">Há erros no tabuleiro ❌</div>
  </div>
</template>

<script setup lang="ts">
import { reactive, ref, onMounted } from 'vue';

const selectRow = ref<number | null>(null);
const selectCol = ref<number | null>(null);
const dificultSelect = ref<string>('facil');
const originalBoard = reactive<number[][]>([]);
const board = reactive<number[][]>([]);
const victory = ref<boolean | null>(null);

function generateBoard(filledCells: number): number[][] {
  const board = Array.from({ length: 9 }, () => Array(9).fill(0));
  let count = 0;

  while (count < filledCells) {
    const row = Math.floor(Math.random() * 9);
    const col = Math.floor(Math.random() * 9);
    const num = Math.floor(Math.random() * 9) + 1;

    if (board[row][col] === 0) {
      board[row][col] = num;
      count++;
    }
  }

  return board;
}

function selectDificult(): number {
  switch (dificultSelect.value) {
    case 'facil':
      return 52;
    case 'medio':
      return 42;
    case 'dificil':
      return 25;
    default:
      return 41;
  }
}

function changeDifficulty(level: string) {
  dificultSelect.value = level;
  resetGame(selectDificult());
}

function resetGame(filledCells: number) {
  const newBoard = generateBoard(filledCells);

  for (let i = 0; i < 9; i++) {
    originalBoard[i] = [...newBoard[i]];
    board[i] = [...newBoard[i]];
  }

  victory.value = null;
  selectRow.value = null;
  selectCol.value = null;
}

function selectCell(row: number, col: number) {
  if (originalBoard[row][col] === 0) {
    selectRow.value = row;
    selectCol.value = col;
    console.log(`Célula fixa clicada em [${row}, ${col}] com valor: ${originalBoard[row][col]}`);
  } else {
    console.log(`Célula fixa clicada em [${row}, ${col}] com valor: ${originalBoard[row][col]}`);
  }
}

function setValue(n: number) {
  if (selectRow.value !== null && selectCol.value !== null) {
    board[selectRow.value][selectCol.value] = n;
  }
}

function deleteValue() {
  setValue(0);
}

function isValidArray(arr: number[]): boolean {
  const nums = arr.filter((n) => n !== 0);
  return new Set(nums).size === nums.length;
}

function getColumn(board: number[][], col: number): number[] {
  return board.map((row) => row[col]);
}

function getBox(board: number[][], boxRow: number, boxCol: number): number[] {
  const box: number[] = [];
  for (let i = boxRow * 3; i < boxRow * 3 + 3; i++) {
    for (let j = boxCol * 3; j < boxCol * 3 + 3; j++) {
      box.push(board[i][j]);
    }
  }
  return box;
}

function validateBoard(): boolean {
  for (let i = 0; i < 9; i++) {
    if (!isValidArray(board[i])) return false;
    if (!isValidArray(getColumn(board, i))) return false;
  }

  for (let row = 0; row < 3; row++) {
    for (let col = 0; col < 3; col++) {
      if (!isValidArray(getBox(board, row, col))) return false;
    }
  }

  return true;
}

function checkVictory() {
  victory.value = validateBoard();
}

onMounted(() => {
  resetGame(selectDificult());
});
</script>

<style scoped>
.sudoku {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16px;
  padding: 16px;
}

.row {
  display: flex;
}

.cell {
  width: 40px;
  height: 40px;
  border: 1px solid #333;
  text-align: center;
  line-height: 40px;
  cursor: pointer;
  user-select: none;
  font-size: 18px;
}

.cell.fixed {
  background-color: #ddd;
  font-weight: bold;
}
.cell.selected {
  background-color: greenyellow;
}

.numbers button,
.controls button,
.dificuldade button {
  margin: 4px;
  padding: 6px 12px;
  font-size: 16px;
  cursor: pointer;
}

.message {
  font-weight: bold;
  font-size: 18px;
}

.success {
  color: green;
}

.error {
  color: red;
}
</style>
