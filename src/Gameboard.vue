<template>
    <div class="game-board">
      <div v-for="(row, rowIndex) in board" :key="rowIndex" class="row">
        <div v-for="(cell, colIndex) in row" :key="colIndex" class="cell" @click="makeMove(rowIndex, colIndex)">
          {{ cell }} 
        </div>
      </div>
      <div class="mess" v-if="winner">{{ message }}</div>
      <div class="reset" v-on:click="reset" v-if="winner"> reset </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        board: [
          ['', '', ''],
          ['', '', ''],
          ['', '', '']
        ],
        currentPlayer: 'X' , 
        nextPlayer: 'Y' , 
        winner: false 
      };
    },
    methods: {
        makeMove(row, col) {
            if (this.board[row][col] == '' ){
                this.$set(this.board[row], col, this.currentPlayer);

                this.winner = this.checkWinner();

                if (this.winner) {
                    if (this.winner == 'draw')
                        this.message = 'draw'
                    else
                        this.message = `${this.winner} wins!`
                } else {
                    const temp = this.currentPlayer;
                    this.currentPlayer = this.nextPlayer;
                    this.nextPlayer = temp;
                }
            } 
        } , 
        checkWinner() {
            for (let i = 0; i < 3; i++) {
            if (
                this.board[i][0] === this.board[i][1] &&
                this.board[i][0] === this.board[i][2] &&
                this.board[i][0] !== ''
            ) {
                return this.board[i][0];
            }
            }

            for (let i = 0; i < 3; i++) {
            if (
                this.board[0][i] === this.board[1][i] &&
                this.board[0][i] === this.board[2][i] &&
                this.board[0][i] !== ''
            ) {
                return this.board[0][i];
            }
            }

            if (
            this.board[0][0] === this.board[1][1] &&
            this.board[0][0] === this.board[2][2] &&
            this.board[0][0] !== ''
            ) {
                return this.board[0][0];
            }

            if (
            this.board[0][2] === this.board[1][1] &&
            this.board[0][2] === this.board[2][0] &&
            this.board[0][2] !== ''
            ) {
            return this.board[0][2];
            }

            if (this.checkDraw()) {
                return 'draw';
            }

            return null;
        } , 
        checkDraw() {
            for (let i = 0; i < 3; i++) {
                for (let j = 0; j < 3; j++) {
                    if (this.board[i][j] === '') {
                    // If there is an empty cell, the game is not a draw
                    return false;
                    }
                }
            }
            return true;
        } , 
        reset() {
            for (let i = 0 ; i < 3 ; i++){
                for (let j = 0 ; j < 3 ; j++){
                    this.board[i][j] = '' 
                }
            }
            this.winner = false 
        }
    }
  };
  </script>
  
  <style scoped>
  .game-board {
    margin: 10vh auto ; 
    width: fit-content ;
  }

  .reset{
    margin-top: 7vh ; 
    font-size: 24px; 
    margin: 7vh auto 0 ; 
    padding: 10px ; 
    border: 1px solid black ; 
    width: fit-content ;
  }

  .mess{
    width: fit-content ; 
    font-size: 26px ; 
    margin: 3vh auto ; 
  }
  
  .row {
    display: flex ; 
    text-align: center; 
  }
  
  .cell {
    width: 200px ; 
    height: 200px ;
    line-height: 200px; 
    font-size: 50px ;  
    border: 1px solid black ; 
    border-collapse:collapse ; 
  }
  </style>
  
  