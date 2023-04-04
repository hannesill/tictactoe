<script setup>
import { ref, computed } from 'vue'

const player = ref('X')
const board = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', ''],
])

const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
]

function getWinner(squares) {
    for (let i = 0; i < lines.length; i++) {
        const line = lines[i];
        if (squares[line[0]] && squares[line[0]] === squares[line[1]] && squares[line[0]] === squares[line[2]]) {
            return squares[line[0]];
        }
    }
    return null;
}
function getWinningLine(squares) {
    for (let i = 0; i < lines.length; i++) {
        const line = lines[i];
        if (squares[line[0]] && squares[line[0]] === squares[line[1]] && squares[line[0]] === squares[line[2]]) {
            return line;
        }
    }
    return null;
}
const winner = computed(() => getWinner(board.value.flat()))
const winningLine = computed(() => getWinningLine(board.value.flat()))

const MakeMove = (row, col) => {
  if (board.value[row][col] !== '' || winner.value) return
  board.value[row][col] = player.value
  player.value = player.value === 'X' ? 'O' : 'X'
}

const ResetGame = () => {
  player.value = 'X'
  board.value = [
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],
  ]
}

</script>

<template>
  <main class="pt-8 text-center dark:bg-gray-800 min-h-screen dark:text-white">
    <h1 class="mb-8 text-3xl font-bold- uppercase">Tic Tac Toe</h1>
    <h3 class="text-xl mb-4">Player {{ player }}'s turn</h3>
    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex">
        <div 
          v-for="(cell, y) in row" 
          :key="y" 
          :class="`border border-white w-20 h-20 hover:bg-gray-700 
          flex items-center icon-text justify-center text-4xl cursor-pointer`"
          @click="MakeMove(x,y)">
          {{ cell }}
        </div>
      </div>
    </div>

    <h2 v-if="winner" class="text-3xl font-bold mb-8">Player {{ winner }} wins!</h2>

    <button 
      class="px-4 py-2 bg-blue-500 text-white rounded font-bold hover:bg-blue-600 duration-300" 
      @click="ResetGame">
      Reset Game
    </button>
    
  </main>
</template>

<style scoped>

</style>
