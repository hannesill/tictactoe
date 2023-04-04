
<script setup>
import { ref, computed } from 'vue'

if (localStorage.theme === 'dark' || (!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
    document.documentElement.classList.add('dark')
} else {
    document.documentElement.classList.remove('dark')
}

const darkMode = ref(false)

function switchAppearanceMode() {
    if (document.documentElement.classList.contains('dark')) {
        document.documentElement.classList.remove('dark')
        darkMode.value = false
    } else {
        document.documentElement.classList.add('dark')
        darkMode.value = true
    }
}

const isDarkMode = computed(() => darkMode.value)

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

const isWinningCell = (row, col) => {
    if (!winningLine.value) return false;
    const index = row * 3 + col;
    return winningLine.value.includes(index);
};

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
        <div class="flex justify-center">
            <div class="w-64 flex justify-between">
                <div class="left-element flex mb-8 items-center">
                    <span class="ml-3 text-m font-medium text-gray-900 dark:text-gray-300">Turn: {{ player }}</span>
                </div>
                <div class="right-element">
                    <label class="flex mb-8 relative items-center cursor-pointer">
                        <input type="checkbox" value="" class="sr-only peer" @change="switchAppearanceMode()">
                        <div class="w-14 h-7 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 dark:peer-focus:ring-blue-800 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-0.5 after:left-[4px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-6 after:w-6 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600"></div>
                        <span class="ml-3 text-m font-medium text-gray-900 dark:text-gray-300">{{ isDarkMode ? 'Light' : 'Dark'}}</span>
                    </label>
                </div>
            </div>
        </div>
        <div class="flex flex-col items-center mb-8">
            <div v-for="(row, x) in board" :key="x" class="flex">
                <div
                    v-for="(cell, y) in row"
                    :key="y"
                    :class="`border w-20 h-20 flex items-center icon-text justify-center text-4xl cursor-pointer
                  border-gray-400 dark:border-white
                  ${isWinningCell(x, y) ? 'bg-green-500' : 'hover:bg-gray-100 dark:hover:bg-gray-700'}`"

                    @click="MakeMove(x,y)">
                    {{ cell }}
                </div>
            </div>
        </div>

        <h2 v-if="winner" class="text-3xl font-bold mb-8">Player {{ winner }} wins!</h2>

        <button
            class="mb-8 px-4 py-2 bg-blue-600 text-white rounded font-bold hover:bg-blue-700 duration-300"
            @click="ResetGame">
            Reset Game
        </button>
    </main>
</template>

