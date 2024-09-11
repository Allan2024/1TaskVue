

<template>
  <div class="game-container">
    <div class="stake-mines-tictactoe">
      <div class="board">
        <div 
          v-for="cell in board" 
          :key="cell.index" 
          :class="['cell', { 'revealed': cell.revealed, 'mine': cell.value === 'X', 'safe': cell.value === 'O' }]"
          @click="handleCellClick(cell.index)"
        >
          <span v-if="cell.revealed">{{ cell.value === 'X' ? '💣' : cell.value === 'O' ? '💎' : '' }}</span>
        </div>
      </div>
      <div class="status">
        <p v-if="winner">{{ winner === 'X' ? '💣' : '💎' }} wins!</p>
        <p v-else-if="isDraw">It's a draw!</p>
        <p v-else>Current player: {{ currentPlayer === 'X' ? '💣' : '💎' }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      board: Array(9).fill().map((_, index) => ({
        index,
        value: null,
        revealed: false
      })),
      currentPlayer: 'X',
      winner: null,
      isDraw: false,
    };
  },
  methods: {
    handleCellClick(index) {
      if (this.board[index].revealed || this.winner) return;
      this.board[index].value = this.currentPlayer;
      this.board[index].revealed = true;
      this.checkWinner();
      this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
    },
    checkWinner() {
      const winningCombinations = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],
        [0, 3, 6], [1, 4, 7], [2, 5, 8],
        [0, 4, 8], [2, 4, 6]
      ];
      for (const combination of winningCombinations) {
        if (
          this.board[combination[0]].value &&
          this.board[combination[0]].value === this.board[combination[1]].value &&
          this.board[combination[1]].value === this.board[combination[2]].value
        ) {
          this.winner = this.board[combination[0]].value;
          return;
        }
      }
      if (this.board.every(cell => cell.revealed)) {
        this.isDraw = true;
      }
    },
  },
};
</script>

<style scoped>
.game-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: linear-gradient(135deg, #1a2a6c, #b21f1f, #fdbb2d);
}

.stake-mines-tictactoe {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: Arial, sans-serif;
  background-color: rgba(255, 255, 255, 0.1);
  padding: 2rem;
  border-radius: 20px;
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
  backdrop-filter: blur(4px);
  border: 1px solid rgba(255, 255, 255, 0.18);
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  margin-bottom: 20px;
}

.cell {
  width: 80px;
  height: 80px;
  background-color: rgba(44, 62, 80, 0.7);
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  font-size: 36px;
  transition: all 0.3s ease;
}

.cell:hover {
  transform: scale(1.05);
  box-shadow: 0 0 15px rgba(255, 255, 255, 0.5);
}

.cell.revealed {
  background-color: rgba(52, 73, 94, 0.8);
}

.cell.revealed.mine {
  background-color: rgba(231, 76, 60, 0.8);
}

.cell.revealed.safe {
  background-color: rgba(46, 204, 113, 0.8);
}

.status {
  font-size: 24px;
  font-weight: bold;
  text-align: center;
  color: white;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}
</style>
