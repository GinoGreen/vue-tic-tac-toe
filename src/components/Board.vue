<template>
   <div id="board">
      <Square
         v-for="(char, boardPosition) in board"
         :key="boardPosition"
         :char="char"
         @chooseXO="userActionOn(boardPosition)"
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
         PLAYERO_WON: 'PLAYERO_WON',
         VACANCY: '',
         TIE: 'TIE',
         outcome: false, //esito
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
      userActionOn(boardPosition) {

         if (this.isValidAction(boardPosition) && this.isGameActive) {
            // aggiorno la board
            this.$set(this.board, boardPosition, this.currentPlayer);
            // gestione della convalida dei risultati
            this.handleResultValidation();
            // cambio turno giocatore
            this.changePlayer();
         }
      },
      resetBoard() {

         this.board = ['', '', '', '', '', '', '', '', ''];
         this.isGameActive = true;
         this.outcome = false;
         this.$emit('announce', {turnFinished: this.outcome});
      
         if (this.currentPlayer === 'O') this.changePlayer();
      },
      changePlayer() {
         // operatore ternario per cambiare da X a O oppure viceversa
         this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
         this.$emit('changePlayer', this.currentPlayer);
      },
      announceOutcome(outcomeType) {

         this.outcome = true;

         this.$emit('announce', {
            turnFinished: this.outcome,
            currentPlayer: this.currentPlayer,
            outcomeType: outcomeType,
         });
      },
      isValidAction(index) {

         if (this.board[index] === 'X' || this.board[index] === 'O') return false;
         // altrimenti
         console.log('true action');
         return true;
      },
      handleResultValidation() {
         
         if (this.thereIsTrisOnBoard()) {
            this.announceOutcome(this.getOutcomeType());
            this.isGameActive = false;
            return;
         }

         if (!this.board.includes(this.VACANCY)) this.announceOutcome(this.TIE);
      },
      thereIsTrisOnBoard() {
         for (const winCondition of this.winningConditions) {
            
            const a = this.board[winCondition[0]];
            const b = this.board[winCondition[1]];
            const c = this.board[winCondition[2]];
            
            if (a === '' || b === '' || c === '') continue; //passo direttamente all'iterazione succesiva

            if (a === b && b === c) {
               return true;
            }
         }
         return false;
      },
      getOutcomeType() {
         return this.currentPlayer === 'X' ? this.PLAYERX_WON : this.PLAYERO_WON
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