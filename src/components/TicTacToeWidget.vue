<template>
  <div class="tic-tac-toe">
    <h1>{{ status }}</h1>
    <div class="board">
      <div
        v-for="(cell, index) in board"
        :key="index"
        class="cell"
        :class="{ 'cell-x': cell === 'X', 'cell-o': cell === 'O' }"
        @click="handleClick(index)"
      >
        {{ cell }}
      </div>
    </div>
    <button v-if="isGameEnded" @click="resetGame">Restart</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      board: ['', '', '', '', '', '', '', '', ''],
      currentPlayer: 'X',
      isGameEnded: false,
      isGameTied: false,
    };
  },
  computed: {
    status() {
      if (this.isGameTied) {
        return "It's a Tie!";
      } else if (this.isGameEnded) {
        return `Player ${this.currentPlayer === 'X' ? 'O' : 'X'} Wins!`;
      }
      return `Player ${this.currentPlayer}'s turn`;
    },
  },
  methods: {
    handleClick(index) {
      if (!this.isGameEnded && this.board[index] === '') {
        this.board[index] = this.currentPlayer;
        this.checkWinner();
        this.checkTie();
        this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
      }
    },
    checkWinner() {
      const winningCombinations = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ];

      for (const combination of winningCombinations) {
        const [a, b, c] = combination;
        if (
          this.board[a] &&
          this.board[a] === this.board[b] &&
          this.board[a] === this.board[c]
        ) {
          this.isGameEnded = true;
          return;
        }
      }
    },
    checkTie() {
      if (!this.board.includes('')) {
        this.isGameEnded = true;
        this.isGameTied = true;
      }
    },
    resetGame() {
      this.board = ['', '', '', '', '', '', '', '', ''];
      this.currentPlayer = 'X';
      this.isGameEnded = false;
      this.isGameTied = false;
    },
  },
};
</script>
<style scoped>
.tic-tac-toe {
  text-align: center;
  padding: 20px;
  color: white;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 10px;
  margin: 20px auto;
  max-width: 300px;
}

.cell {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #e8e8e8;
  color: #333;
  font-size: 3rem;
  height: 100px;
  cursor: pointer;
}

.cell:hover {
  background-color: #d1d1d1;
}

.cell-x {
  color: #ff7f50;
}

.cell-o {
  color: #1e90ff;
}

button {
  display: block;
  margin: 20px auto;
  padding: 10px 20px;
  font-size: 1rem;
  background-color: #4caf50;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}
</style>
