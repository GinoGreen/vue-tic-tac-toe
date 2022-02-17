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
         board: Array(9).fill(null),
         isGameActive: true,
         currentPlayer: 'X',
         VACANCY: null,
         TIE: 'TIE',
         PLAYERX_WON: 'PLAYERX_WON',
         PLAYERO_WON: 'PLAYERO_WON',
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
      resetFullGame() {

         this.board = Array(9).fill(null);
         this.isGameActive = true;
         this.outcome = false;
         this.$emit('sendDetailsAnnounce', {outcome: this.outcome});
      
         if (this.currentPlayer === 'O') this.changePlayer();
      },
      changePlayer() {
         // operatore ternario per cambiare da X a O oppure viceversa
         this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
         this.$emit('changePlayer', this.currentPlayer);
      },
      announceOutcome(outcomeType) {

         this.outcome = true;

         this.$emit('sendDetailsAnnounce', {
            currentPlayer: this.currentPlayer,
            outcome: this.outcome,
            outcomeType: outcomeType,
         });
      },
      isValidAction(index) {

         if (this.board[index] === 'X' || this.board[index] === 'O') return false;
         return true;
      },
      handleResultValidation() {
         
         if (this.thereIsTrisOnBoard()) {
            this.announceOutcome(this.getWinningPlayer());
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
            
            if (a === null || b === null || c === null) continue; //passo direttamente all'iterazione succesiva

            if (a === b && b === c) {
               return true;
            }
         }
         return false;
      },
      getWinningPlayer() {
         return this.currentPlayer === 'X' ? this.PLAYERX_WON : this.PLAYERO_WON;
      }
   },
   mounted() {
      this.$root.$on('Board', () => {
         this.resetFullGame();
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