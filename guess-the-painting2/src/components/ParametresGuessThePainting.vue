<template>
   <div id="listeParam">
      <div class="parametre">
         <div id="nbPainting">
            <h2>Nombre de peintures</h2>
            <div class="nbPainting">
               <button @click="decrementPainting">-</button>
               <p>{{ nbPainting }}</p>
               <button @click="incrementPainting">+</button>
            </div>
         </div>
      </div>
      <div class="parametre">
         <div id="duree">
            <h2>Durée</h2>
            <div class="duree">
               <button @click="setDuration(30)">30sec</button>
               <button @click="setDuration(45)">45sec</button>
               <button @click="setDuration(60)">60sec</button>
               <button @click="setDuration(null)">Infini</button>
            </div>
         </div>
      </div>
      <div id="bouton">
         <a href="#/GuessThePainting">
         <params @start-game="startGame"></params>
         </a>
      </div>
   </div>
</template>
 
<script>
export default {
   data() {
      return {
         nbPainting: 10,
         duration: null
      };
   },
   methods: {
      incrementPainting() {
         this.nbPainting++;
      },
      decrementPainting() {
         if (this.nbPainting > 1) {
            this.nbPainting--;
         }
      },
      setDuration(value) {
         this.duration = value;
      },
      startGame() {
         this.$emit("start-game", {
            nbPainting: this.nbPainting,
            duration: this.duration
         });
      }
   }
};
</script>
 