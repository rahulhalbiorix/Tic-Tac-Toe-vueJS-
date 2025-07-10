<template>
  <div class="tic-tac-toe">
    <div class="game-card">
      <header class="game-header">
        <h1>Tic-Tac-Toe</h1>
      </header>

      <div class="turn-message" v-if="!isGameEnded">
        <h3>Now It's {{ curruentPlayer }}'s Turn!</h3>
      </div>

      <div class="game-board" v-for="(row, rowIndex) in gameBoard" :key="rowIndex">
        <div
          class="game-cell"
          v-for="(col, colIndex) in row"
          :key="colIndex"
          @click="playerMove(rowIndex, colIndex)"
        >
          {{ col }}
        </div>
      </div>

      <div class="game-footer">
        <button class="refresh-btn" @click="restartTheGame">Restart Game</button>
        <div class="winner-message" v-if="isGameEnded">
          <h2 v-if="winner !== 'tie'">{{ winner }} is the Winner!</h2>
          <h2 v-else>It's a Tie!</h2>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, watch } from 'vue'

const gameBoard = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', ''],
])

console.log('Game-Board', gameBoard)

const player = ref<string[]>(['X', 'O'])

let curruentPlayer = player.value[0]

console.log('Now curruent player is ', curruentPlayer)
console.log('list of player', player.value)

const isGameEnded = ref(false)

const winner = ref<string>()

function playerMove(rowIndex: number, colIndex: number) {
  if (isGameEnded.value) return

  gameBoard.value[rowIndex][colIndex] = curruentPlayer

  if (curruentPlayer == player.value[0]) {
    curruentPlayer = player.value[1]
  } else {
    curruentPlayer = player.value[0]
  }
}

watch(
  gameBoard,
  () => {
    console.log('Watch property is work')
    checkWin()
  },
  { deep: true },
)

function checkWin() {
  console.log('Function cjeck win is worked')

  const newGameBoard = gameBoard.value.flat()
  console.log(newGameBoard)

  //for row
  for (let r = 0; r < 3; r++) {
    let r1 = newGameBoard.slice(r * 3, r * 3 + 3)

    if (r1.every((item) => item == 'X')) {
      console.log('row', r1)
      winner.value = 'X'
      isGameEnded.value = true
      return
    }

    if (r1.every((item) => item == 'O')) {
      winner.value = 'O'
      isGameEnded.value = true
      return
    }
  }

  //for column

  for (let c = 0; c < 3; c++) {
    let c1 = [newGameBoard[c], newGameBoard[c + 3], newGameBoard[c + 6]]

    if (c1.every((item) => item == 'X')) {
      winner.value = 'X'
      isGameEnded.value = true
      return
    }

    if (c1.every((item) => item == 'O')) {
      winner.value = 'O'
      isGameEnded.value = true
      return
    }
  }

  //for diagonalrow

  let diagonalLeftToRight = [newGameBoard[0], newGameBoard[4], newGameBoard[8]]
  let diagonalRightToLeft = [newGameBoard[2], newGameBoard[4], newGameBoard[6]]

  if (diagonalLeftToRight.every((item) => item == 'X')) {
    winner.value = 'X'
    isGameEnded.value = true
    return
  }

  if (diagonalLeftToRight.every((item) => item == 'O')) {
    winner.value = 'O'
    isGameEnded.value = true
    return
  }

  if (diagonalRightToLeft.every((item) => item == 'X')) {
    winner.value = 'X'
    isGameEnded.value = true
    return
  }
  if (diagonalRightToLeft.every((item) => item == 'O')) {
    winner.value = 'O'
    isGameEnded.value = true
    return
  }

  if (newGameBoard.every((item) => item !== '')) {
    winner.value = 'tie'
    isGameEnded.value = true
    return
  }

  return
}

function restartTheGame() {
  console.log('Resdtart the ga,e is work!...')

  gameBoard.value = [
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],
  ]

  curruentPlayer = player.value[0]

  isGameEnded.value = false

  winner.value = ''
}
</script>

<style scoped>
/* Main Container */
.tic-tac-toe {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  padding: 20px;
}

.game-card {
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  width: 300px;
  padding: 20px;
  text-align: center;
}

.game-header h1 {
  font-size: 2rem;
  color: #333;
}

.turn-message h3 {
  font-size: 1.2rem;
  color: #555;
  margin: 10px 0;
  font-weight: bold;
}

/* Game Board */
.game-board {
  display: grid;
  grid-template-columns: repeat(3, minmax(80px, 1fr));
  border: 2px solid black;
}

.game-cell {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 80px;
  background-color: #efefef;
  font-size: 2rem;
  font-weight: bold;
  color: #333;
  cursor: pointer;
  border: 2px solid black;
  transition: background-color 0.3s ease;
}

.game-cell:hover {
  background-color: #ddd;
}

/* Footer Section */
.game-footer {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 10px;
}

.refresh-btn {
  padding: 10px 20px;
  background-color: #009688;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  margin-bottom: 10px;
}

.refresh-btn:hover {
  background-color: #00796b;
}

.winner-message h2 {
  font-size: 1.5rem;
  color: #4caf50;
  font-weight: bold;
}

/* Mobile Responsive */
@media screen and (max-width: 400px) {
  .game-card {
    width: 260px;
  }

  .game-cell {
    width: 60px;
    height: 60px;
    font-size: 1.5rem;
  }

  .refresh-btn {
    padding: 8px 16px;
  }
}
</style>
