<template>
  <div id="app">
    <h1>Vue Tic Tac Toe</h1>
    <div>
      <p>Player O: {{ playerOneScore }} </p>

      Player X: {{ playerTwoScore }}
    </div>
    <div id="tic-tac-toe-container">
      <div
        class="tic-tac-row"
        v-for="(row, row_index) in grid"
        :key="`row-${row_index}`"
      >
        <div
          class="cell tic-tac-col"
          v-for="(col, col_index) in row"
          :key="`col-${col_index}`"
          @click="updateGrid(row_index, col_index)"
        >
          {{ col }}
        </div>
      </div>
    </div>

    <div id="menu-container">
      <div v-if="winBanner">
        {{ winBanner }}
      </div>
      <div id="play-button" v-if="!gameStarted" @click="startGame">
        PLAY
      </div>

      <div v-if="gameStarted">
        Player Turn: {{ playerName }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',

  data: function () {
    return {
      playerOneScore: 0,
      playerTwoScore: 0,
      gameStarted: false,
      players: ["X", "O"],
      playerOneTurn: true,
      winBanner: null,
      grid: [
        [null, null, null],
        [null, null, null],
        [null, null, null]
      ]
    }
  },

  computed: {
    playerName () {
      return this.playerOneTurn ? "O" : "X"
    }
  },

  methods: {
    startGame () {
      this.gameStarted = true

      this.grid =
        [
          [null, null, null],
          [null, null, null],
          [null, null, null]
        ]
      this.winBanner = null
      this.playerOneTurn = true
    },

    initializeGame () {

    },

    updateGrid (x, y) {
      if (!this.gameStarted) return

      this.grid[x][y] = this.playerOneTurn ? "O" : "X"

      if (this.hasWinner()) {
        this.gameStarted = false
        this.winBanner = `Player ${this.playerName} has won!`
        if (this.playerOneTurn) {
          this.playerOneScore++
        } else {
          this.playerTwoScore++
        }
      } else {
        this.playerOneTurn = !this.playerOneTurn
      }
    },

    hasWinner () {
      // check for vertical match
      for(let i = 0; i < 3; i++) {
        if (
          this.grid[0][i] && this.grid[1][i] && this.grid[2][i] &&
          this.grid[0][i] == this.grid[1][i] && this.grid[0][i] == this.grid[2][i]
        ) {
          return true
        }
      }

      // check for horizontal match
      for(let x = 0; x < 3; x++) {
        if (
          this.grid[x][0] && this.grid[x][1] && this.grid[x][2] &&
          this.grid[x][0] == this.grid[x][1] && this.grid[x][0] == this.grid[x][2]
        ) {
          return true
        }
      }

      //check for diagonal
      if (
        (this.grid[0][0] && this.grid[1][1] && this.grid[2][2] &&
        this.grid[0][0] == this.grid[1][1] && this.grid[0][0] == this.grid[2][2]) ||
        (this.grid[0][2] && this.grid[1][1] && this.grid[2][0] &&
        this.grid[0][2] == this.grid[1][1] && this.grid[0][2] == this.grid[2][0])
      ) {
        return true
      }

      return false
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#tic-tac-toe-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.tic-tac-row {
  display: flex;
  flex-direction: row;
}

.cell {
  height: 100px;
  width: 100px;
  border: 2px solid black;
}

#menu-container {
  margin-top: 2em
}

#play-button {
  background-color: #25a025;
  color: white;
  width: 100px;
  margin: auto;
  padding: 1em;
  border-radius: 1em;
}

#play-button:hover {
  cursor: pointer;
  background-color: #1f521f;
}
</style>
