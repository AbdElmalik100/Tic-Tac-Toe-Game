<script setup>
import { ref, computed } from 'vue'

const player = ref("X")

const counter = ref(0)
const draw = ref(false)

const board = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', ''],
])

let calculateWinnder = (squares) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ]

  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i]
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a]
    }
  }
  return null
}

const winner = computed(() => calculateWinnder(board.value.flat()))

const makeMove = (x, y) => {
  if (board.value[x][y] === "") {
    counter.value++
    if (counter.value === 9) {
      // This Delay For Getting The Value Of Winner .. We Dont Want It Null :)
      setTimeout(() => {
        winner.value === null ? draw.value = true : ''
      }, 100)
    }
  }
  if (winner.value) return

  if (board.value[x][y] !== "") return

  board.value[x][y] = player.value

  player.value = player.value === "X" ? "O" : "X"
}

let resetGame = () => {
  board.value = [
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],
  ]
  player.value = "X"
  counter.value = 0
  draw.value = false
}

</script>

<template>
  <main class="py-3 text-center min-vh-100 bg-dark text-light">
    <h1 class="mb-4 mt-0 fs-1 fw-bold text-uppercase">Tic Tac Toe</h1>
    <h3 class="fs-3 my-4">Player {{ player }}'s Turn</h3>
    <div class="d-flex flex-column align-items-center mb-4">
      <div v-for="(row, x) in board" :key="x" class="d-flex">
        <div v-for="(cell, y) in row" :key="y" @click="makeMove(x, y)" style="width: 100px; height: 100px;"
          class="cell border border-light fs-1 d-flex align-items-center justify-content-center">
          <i
            :class="`${cell === 'X' ? 'fa-solid fa-xmark text-info' : cell === 'O' ? 'fa-regular fa-circle text-danger' : ''}`"></i>
        </div>
      </div>
    </div>
    <h2 v-if="winner" class="fs-1 fw-bold mb-3">Player {{ winner }} Wins!</h2>
    <h2 v-if="draw" class="fs-1 fw-bold mb-3">Draw</h2>
    <button class="border-0 py-3 px-5 text-white text-uppercase fw-bold rounded" @click="resetGame">Reset Game</button>
</main>
</template>

<style lang="scss">
.cell {
  cursor: pointer;
  transition: 0.1s;

  &:hover {
    background-color: grey;
  }
}

button {
  transition: 0.1s;
  background-color: palevioletred;

  &:hover {
    background-color: pink;
  }
}
</style>
