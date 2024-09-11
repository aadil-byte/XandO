<template>
  <div id="app">
    <div class="game-container">
      <h1>Tic-Tac-Toe</h1>
      <div class="board">
        <div
          v-for="(cell, index) in board"
          :key="index"
          :class="['cell', cell]"
          @click="makeMove(index)"
        >
          {{ cell }}
        </div>
      </div>
      <div class="status">
        <p>{{ statusMessage }}</p>
      </div>
      <button @click="resetGame">Reset Game</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      board: Array(9).fill(null),
      currentPlayer: 'X',
      winner: null,
      statusMessage: 'Player X\'s Turn',
    };
  },
  methods: {
    makeMove(index) {
      if (this.board[index] || this.winner) return;

      this.$set(this.board, index, this.currentPlayer);

      if (this.checkWinner()) {
        this.winner = this.currentPlayer;
        this.statusMessage = `Player ${this.currentPlayer} Wins!`;
      } else if (this.board.every(cell => cell)) {
        this.statusMessage = 'It\'s a Tie!';
      } else {
        this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
        this.statusMessage = `Player ${this.currentPlayer}'s Turn`;
      }
    },
    checkWinner() {
      const winPatterns = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ];
      for (const pattern of winPatterns) {
        const [a, b, c] = pattern;
        if (this.board[a] && this.board[a] === this.board[b] && this.board[a] === this.board[c]) {
          return true;
        }
      }
      return false;
    },
    resetGame() {
      this.board = Array(9).fill(null);
      this.currentPlayer = 'X';
      this.winner = null;
      this.statusMessage = 'Player X\'s Turn';
    }
  }
};
</script>

<style scoped>
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  margin-top: 50px;
}

.game-container {
  display: inline-block;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-template-rows: repeat(3, 100px);
  gap: 5px;
  margin-bottom: 20px;
}

.cell {
  width: 100px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
  border: 1px solid #000;
  cursor: pointer;
}

.cell.X {
  color: #ff5722;
}

.cell.O {
  color: #2196f3;
}

.status {
  margin-bottom: 20px;
}

button {
  padding: 10px 20px;
  font-size: 1rem;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}
</style>
g