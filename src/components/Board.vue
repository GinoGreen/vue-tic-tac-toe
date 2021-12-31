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
         PLAYERX_WON: 'PLAYERX_WON',
         PLAYERY_WON: 'PLAYERY_WON',
         TIE: 'TIE',
         upshot: false, //esito
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

         if (this.isValidAction(boardIndex) && this.isGameActive) {
            // aggiorno la board
            this.$set(this.board, boardIndex, this.currentPlayer);
            // check azione valida
            this.handleResultValidation();
            // cambio turno giocatore
            this.changePlayer();
         }
      },
      resetBoard() {

         this.board = ['', '', '', '', '', '', '', '', ''];
         this.isGameActive = true;
         this.upshot = false;
         this.$emit('announce', {turnFinished: this.upshot});
      
         if (this.currentPlayer === 'O') this.changePlayer;
      },
      changePlayer() {
         // operatore ternario per cambiare da X a O oppure viceversa
         this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
         this.$emit('changePlayer', this.currentPlayer)
      },
      announce(type) {
         // switch per annunciare vincitore/pareggio
         this.upshot = true;
         this.$emit('announce', {
            
            turnFinished: this.upshot,
            currentPlayer: this.currentPlayer,
            type: type,
         });


      },
      isValidAction(index) {

         if (this.board[index] === 'X' || this.board[index] === 'O') return false;
         // altrimenti
         return true;
      },
      handleResultValidation() {

         let roundWon = false;

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

         if (roundWon) {
            this.announce(this.currentPlayer === 'X' ? this.PLAYERX_WON : this.PLAYERY_WON);
            this.isGameActive = false;
            return;
         }

         if (!this.board.includes('')) this.announce(this.TIE);
      }
   },
   mounted() {
      this.$root.$on('Board', () => {
         this.resetBoard();
      });
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