<template>
   <div id="activePlayer">
      <h2>Turno di:</h2>
      <div class="btns">
         <button class="btn-custom">X</button>
         <button class="btn-custom">O</button>
         <div class="showChange" :class="[playerChanged === 'X' ? 'xTurn' : 'oTurn']"></div>
      </div>
   </div>
</template>

<script>
export default {
   name: 'ActivePlayer',
   props: {
      playerChanged: String,
   }
}
</script>

<style lang="scss">

@import '../assets/style/vars.scss';
@import '../assets/style/mixins.scss';

   #activePlayer {
      height: 142px;
      padding: 20px 0;

      h2 {
         font-size: 1.2em;
         text-align: center;
      }

      .showChange {
         width: 50%;
         height: 60px;

         position: absolute;
         top: 0;
         
         border-radius: 50px;
         background: $primary-color;
         z-index: 100;
      }
      .xTurn {
         @include changePlayer('showChangeX'){
            from { right: 0 }
            to { right: 50% }
         }
         right: 50%;
      }
      .oTurn {
         @include changePlayer('showChangeO'){
            from { left: 0 }
            to { left: 50% }
         }
         left: 50%;
      }
   }
</style>