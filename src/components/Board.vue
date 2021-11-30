<template>
   <div id="board">
      <Square
         v-for="(char, index) in board"
         :key="index"
         :char="char"
         @chooseXO="userAction(index)"
      />
   </div>
</template>

<script>

import Square from './Square.vue';

export default {
   name: 'Board',
   components: {
      Square
   },
   data(){
      return{
         board: ['', '', '', '', '', '', '', '', ''],
         currentPlayer: 'X',
         isGameActive: true,
         /*
               Indici dentro il board:
               [0] [1] [2]
               [3] [4] [5]
               [6] [7] [8]
         */
         winningConditions: [
            [0, 1, 2],
            [3, 4, 5],
            [6, 7, 8],
            [0, 3, 6],
            [1, 4, 7],
            [2, 5, 8],
            [0, 4, 8],
            [2, 4, 6]
         ]
      }
   },
   methods: {
      userAction(boardIndex) {

         // if (isValidAction(choose) && this.isGameActive) {
            // aggiorno la board
            this.$set(this.board, boardIndex, this.currentPlayer);
            console.log('board', this.board);
            // cambio turno giocatore
            this.changePlayer();
            this.handleResultValidation();
         // }
      },
      changePlayer() {
         // operatore ternario per cambiare da X a O oppure viceversa
         this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
      },
      announce() {

      },
      handleResultValidation() {
         let roundWon = false;
         console.log('roundWon prima ciclo', roundWon);
         for (const winCondition of this.winningConditions) {
            
            const a = this.board[winCondition[0]];
            const b = this.board[winCondition[1]];
            const c = this.board[winCondition[2]];
            
            if (a === '' || b === '' || c === '') continue; //passo direttamente all'iterazione succesiva

            if (a === b && b === c) {
               roundWon = true;
               break; //esco effettivamente dal ciclo
            }
         }


         console.log('roundWon dopo ciclo', roundWon);

         if (roundWon) {
            this.announce();
         }
      }
   }
}
</script>

<style lang="scss">

#board {
   width: 400px;
   height: 400px;
   display: flex;
   flex-wrap: wrap;
}
</style>