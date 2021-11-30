<template>
   <div 
      class="square"
      @click="$emit('chooseXO')"
   >
      {{ char }}
   </div>
</template>

<script>
export default {
   name: 'Square',
   props: {
      char: String
   },
   methods: {
      
   }
}
</script>

<style lang="scss">

@import '../assets/style/vars.scss';

.square {
   height: calc(100% / 3);
   width: calc(100% / 3);
   
   display: flex;
   justify-content: center;
   align-items: center;

   color: #29cde0;
   font-size: 80px;
   box-shadow: 2px 2px 2px rgba(255, 255, 255, .26),
               -2px -2px 5px rgba(0, 0, 0, .87);
   cursor: pointer;
}
</style>