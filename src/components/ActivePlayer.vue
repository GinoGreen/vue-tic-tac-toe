<template>
   <div id="activePlayer">
      <h2>Turno di:</h2>
      <div class="btns">
         <button>X</button>
         <button>O</button>
         <div id="showChange"></div>
      </div>
   </div>
</template>

<script>
export default {
   name: 'ActivePlayer'
}
</script>

<style lang="scss">

@import '../assets/style/vars.scss';

   #activePlayer {
      padding: 20px 0;

      h2 {
         font-size: 1.2em;
         text-align: center;
      }

      .btns {
         height: 60px;
         margin: 10px 0;

         display: flex;
         position: relative;

         border-radius: 50px;
         box-shadow: 
            2px 2px 2px rgba(255, 255, 255, .26),
            -2px -2px 5px rgba(0, 0, 0, .87);
         overflow: hidden;

         button {
            width: 50%;

            color: #fff;
            font-size: 1.4em;

            border: none;
            background-color: transparent;

            z-index: 200;
         }

         #showChange {
            width: 50%;
            height: 60px;

            position: absolute;
            top: 0;
            left: 0;
            
            border-radius: 50px;
            background: $primary-color;
            z-index: 100;
            transition: 0.2s;
         }
      }
   }
</style>