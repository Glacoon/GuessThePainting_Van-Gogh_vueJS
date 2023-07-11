<template>
   <div id="headGuess">
      <!--Afficher le nom du peintre-->
      <a href="/">
      <h1>Guess the painting {{ PainterName }} </h1>
   </a>
   </div>
</template>
<script>
export default {
   data() {
      return {
         PainterName: "Vincent Van Gogh"
      }
   }
   
};
</script>
<style lang="scss">

#headGuess a {
   text-decoration: none;
   color: #4b627a;
   transition: color 0.3s ease-in-out;
   &:hover {
      color: white;
   }
}

</style>