<script setup>
import { reactive } from "vue";
import Cell from "./Cell.vue";

const boardSize = 9;
defineProps({
  msg: {
    type: String,
    required: true,
  },
});

const state = reactive({
  selectedRow: -1,
  selectedCol: -1,
  board: [],
});
state.board = initBoard();

function select({ row, col }) {
  state.board[row][col].selected = true;
  if (state.selectedRow != -1) {
    state.board[state.selectedRow][state.selectedCol].selected = false;
  }
  state.selectedRow = row;
  state.selectedCol = col;
}

function enter(n) {
  if (state.selectedRow != -1 && state.board[state.selectedRow][state.selectedCol].options[n - 1]) {
    state.board[state.selectedRow][state.selectedCol].content = `${n}`;
    updateOptions(state.selectedRow, state.selectedCol, n);
    detectSafeOption();
  }
}

function initBoard() {
  const board = [];
  let i = 0;
  const fullOptions = [];
  for (let j = 0; j < 9; j++) {
    fullOptions.push(true);
  }
  for (let row = 0; row < boardSize; row++) {
    const boardRow = [];
    for (let col = 0; col < boardSize; col++) {
      boardRow.push({ id: i++, row, col, content: "", selected: false, options: [...fullOptions] });
    }
    board.push(boardRow);
  }
  return board;
}

function updateOptions(row, col, value) {
  // Column
  for (let i = 0; i < boardSize; i++) {
    if (i != row) {
      state.board[i][col].options[value - 1] = false;
    }
  }
  // Row
  for (let i = 0; i < boardSize; i++) {
    if (i != col) {
      state.board[row][i].options[value - 1] = false;
    }
  }
  // Block
  const rowBlock = Math.floor(row / 3);
  const colBlock = Math.floor(col / 3);
  for (let r = 0; r < 3; r++) {
    const rowCell = rowBlock * 3 + r;
    for (let c = 0; c < 3; c++) {
      const colCell = colBlock * 3 + c;
      if ((rowCell != row) || (colCell != col)) {
        state.board[rowCell][colCell].options[value - 1] = false;
      }
    }
  }
}

function detectSafeOption() {
  for (let r = 0; r < boardSize; r++) {
    for (let c = 0; c < boardSize; c++) {
      const nbOfOptions = state.board[r][c].options.filter(i => i).length;
      if ((nbOfOptions === 1) && state.board[r][c].content === "") {
        console.log(`Safe option at row ${r}, col ${c}`);
        console.log(state.board[r][c].options.filter(i => i));
      }
    }
  }
}
</script>

<template>
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
    <h3>Jouons à Sudoku.</h3>
    <table>
      <tr v-for="(row, index) in state.board" :key="index">
        <td v-for="cell in row" :key="cell.id">
          <Cell v-bind="cell" @select="select" />
        </td>
      </tr>
    </table>
    <p>
      <button v-for="i in 9" :key="i" @click="enter(i)">
        {{ i }}
      </button>
    </p>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

table {
  background-color: darkgray;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
