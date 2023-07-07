<template>
  <div class="snake-game-widget">
    <h2>Snake Game</h2>
    <div class="board">
      <div
        v-for="(cell, index) in board"
        :key="index"
        :class="{ snake: isSnakeCell(index), food: isFoodCell(index) }"
      ></div>
    </div>
    <div class="controls">
      <button @click="changeDirection('up')">Up</button>
      <div>
        <button @click="changeDirection('left')">Left</button>
        <button @click="changeDirection('right')">Right</button>
      </div>
      <button @click="changeDirection('down')">Down</button>
    </div>
    <button @click="startGame" :disabled="isGameStarted">Start</button>
    <button @click="resetGame">Reset</button>
    <p>{{ status }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      boardSize: 15,
      board: [],
      snake: [],
      food: null,
      direction: 'right',
      isGameStarted: false,
      isGameOver: false,
    };
  },
  computed: {
    status() {
      if (this.isGameOver) {
        return 'Game Over!';
      } else if (this.isGameStarted) {
        return 'Playing...';
      } else {
        return 'Click Start to Begin';
      }
    },
  },
  mounted() {
    this.initializeBoard();
  },
  methods: {
    initializeBoard() {
      this.board = Array(this.boardSize * this.boardSize).fill('');
      this.snake = [0];
      this.food = null;
      this.direction = 'right';
      this.isGameStarted = false;
      this.isGameOver = false;
    },
    isSnakeCell(index) {
      return this.snake.includes(index);
    },
    isFoodCell(index) {
      return this.food === index;
    },
    startGame() {
      this.isGameStarted = true;
      this.placeFood();
      this.moveSnake();
    },
    resetGame() {
      this.initializeBoard();
    },
    placeFood() {
      const emptyCells = this.board
        .map((_, index) => index)
        .filter(index => !this.snake.includes(index));
      this.food = emptyCells[Math.floor(Math.random() * emptyCells.length)];
    },
    moveSnake() {
      const head = this.snake[0];
      let newHead;
      switch (this.direction) {
        case 'up':
          newHead = head - this.boardSize;
          break;
        case 'down':
          newHead = head + this.boardSize;
          break;
        case 'left':
          newHead = head - 1;
          break;
        case 'right':
          newHead = head + 1;
          break;
      }

      if (this.isOutOfBounds(newHead) || this.isSnakeCollision(newHead)) {
        this.isGameOver = true;
        return;
      }

      this.snake.unshift(newHead);

      if (newHead === this.food) {
        this.placeFood();
      } else {
        this.snake.pop();
      }

      setTimeout(() => {
        if (this.isGameStarted) {
          this.moveSnake();
        }
      }, 300);
    },
    isOutOfBounds(index) {
      return (
        index < 0 ||
        index >= this.boardSize * this.boardSize ||
        (index % this.boardSize === 0 && this.direction === 'left') ||
        ((index + 1) % this.boardSize === 0 && this.direction === 'right')
      );
    },
    isSnakeCollision(index) {
      return this.snake.includes(index);
    },
    changeDirection(newDirection) {
      if (
        (this.direction === 'up' && newDirection === 'down') ||
        (this.direction === 'down' && newDirection === 'up') ||
        (this.direction === 'left' && newDirection === 'right') ||
        (this.direction === 'right' && newDirection === 'left')
      ) {
        return;
      }
      this.direction = newDirection;
    },
  },
};
</script>

<style scoped>
.snake-game-widget {
  border: 1px solid #ccc;
  padding: 20px;
  margin-bottom: 20px;
}

.board {
  display: grid;
  grid-template-columns: repeat(15, 1fr);
  grid-gap: 2px;
  margin-bottom: 10px;
}

.board div {
  background-color: #ccc;
  height: 20px;
}

.board .snake {
  background-color: #333;
}

.board .food {
  background-color: #ff5722;
}

.controls {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 10px;
}

.controls button {
  margin-top: 5px;
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 50%;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.controls button:hover {
  background-color: #0056b3;
}

button {
  margin-top: 5px;
  padding: 10px 20px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 50%;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #45a049;
}
</style>
