<template>
  <div id="wrapper">
    <div class="container"> 

      <Title/>

      <ActivePlayer/>

      <Board/>

    </div>
  </div>
</template>

<script>

import Title from './components/Title.vue';
import ActivePlayer from './components/ActivePlayer.vue';
import Board from './components/Board.vue';

export default {
  name: 'App',
  components: {
    Title,
    ActivePlayer,
    Board
  }
}
</script>

<style lang="scss">

@import './assets/style/vars.scss';
@import './assets/style/generals.scss';
@import './assets/style/mixins.scss';

  #wrapper {
    height: 100vh;

    @include center();
    background-color: #131419;

    .container {
      padding: 30px;

      border-radius: 20px;
      background: linear-gradient(to bottom, #2b2d47 10%, #181926);
      box-shadow: 
        inset 1px 2px 0px rgba(255, 255, 255, .18),
        inset -2px -2px 2px rgba(0, 0, 0, .65);
    }
  }
</style>
