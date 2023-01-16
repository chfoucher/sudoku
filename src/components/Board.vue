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
let i = 0;
for (let row = 0; row < boardSize; row++) {
  const boardRow = [];
  for (let col = 0; col < boardSize; col++) {
    boardRow.push({ id: i++, row, col, content: "", selected: false });
  }
  state.board.push(boardRow);
}

function select({ row, col }) {
  state.board[row][col].selected = true;
  if (state.selectedRow != -1) {
    state.board[state.selectedRow][state.selectedCol].selected = false;
  }
  state.selectedRow = row;
  state.selectedCol = col;
}

function enter(n) {
  if (state.selectedRow != -1) {
    state.board[state.selectedRow][state.selectedCol].content = `${n}`;
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
