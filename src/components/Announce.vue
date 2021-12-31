<template>
   <div id="announce">

      <!-- pareggio -->
      <div 
         class="upshot"
         v-if="infoAnnounce.type === 'TIE'"
      >
         <h4>Pareggio!</h4>
      </div>

      <!-- //vincita -->
      <div 
         class="upshot"
         v-else
      >
         <h4>Il vincitore é </h4>
         <div class="player">{{ infoAnnounce.currentPlayer }}</div>
      </div>

      <!-- reset button -->
      <div 
         class="btns"
         @click="resetBoard"
      >
         <button class="btn-custom">RESET</button>
      </div>
   </div>
</template>

<script>
export default {
   name: 'Announce',
   props: {
      infoAnnounce: Object,
   },
   methods: {
      resetBoard(){
         this.$root.$emit('Board');
      },
   }
}
</script>

<style lang="scss">
@import '../assets/style/vars.scss';
@import '../assets/style/mixins.scss';

   #announce {
      height: 142px;
      @include center;
      flex-direction: column;
      font-size: 2.2em;
      
      .upshot {
         @include center;
         .player {
            @include center;
            margin: 0 15px;
            height: 60px;
            width: 60px;
            font-size: 1.3em;
            border-radius: 40%;
            background-color: $primary-color;
         }
      }
      
      .btns {
         height: auto;
         margin: 0;
         margin-top: .5rem;
         padding: 6px 40px;
         transition: .2s all;
         cursor: pointer;
         &:hover {
            background-color: $primary-color;
         }
         .btn-custom {
            font-size: 1.5rem;
            cursor: pointer;
         }
      }
   }

</style>