<template>
  <div class="game">
    <h1>Tic Tac Toe</h1>
    <div class="board">
      <div
        v-for="(value, index) in board"
        :key="index"
        class="square"
        :class="{ winning: winningCombination.includes(index) }"
        @click="makeMove(index)"
      >
        {{ value }}
      </div>
    </div>
    <p v-if="winner" class="status">{{ winner }} wins!</p>
    <p v-else-if="isDraw" class="status">It's a draw!</p>
    <button @click="resetGame">Restart Game</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      board: Array(9).fill(null),
      currentPlayer: "X",
      winner: null,
      winningCombination: [],
    };
  },
  computed: {
    isDraw() {
      return this.board.every((cell) => cell) && !this.winner;
    },
  },
  methods: {
    makeMove(index) {
      if (this.board[index] || this.winner) return; // Prevent overwriting or moves after game ends

      // Update board with current player's move
      this.$set(this.board, index, this.currentPlayer);

      // Check for a winner
      if (this.checkWinner()) {
        this.winner = this.currentPlayer;
        return;
      }

      // Switch player
      this.currentPlayer = this.currentPlayer === "X" ? "O" : "X";
    },
    checkWinner() {
      const winningPatterns = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ];

      for (let pattern of winningPatterns) {
        const [a, b, c] = pattern;
        if (
          this.board[a] &&
          this.board[a] === this.board[b] &&
          this.board[a] === this.board[c]
        ) {
          this.winningCombination = pattern;
          return true;
        }
      }
      return false;
    },
    resetGame() {
      this.board = Array(9).fill(null);
      this.currentPlayer = "X";
      this.winner = null;
      this.winningCombination = [];
    },
  },
};
</script>

<style scoped>
.game {
  text-align: center;
  margin-top: 50px;
  font-family: Arial, sans-serif;
}
.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  gap: 5px;
  margin: 20px auto;
  justify-content: center;
}
.square {
  width: 100px;
  height: 100px;
  font-size: 2rem;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #f4f4f4;
  border: 1px solid #ccc;
  cursor: pointer;
  user-select: none;
}
.square.winning {
  background-color: #4caf50;
  color: white;
}
.status {
  margin-top: 20px;
  font-size: 1.2rem;
  font-weight: bold;
}
button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 1rem;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
button:hover {
  background-color: #0056b3;
}
</style>
