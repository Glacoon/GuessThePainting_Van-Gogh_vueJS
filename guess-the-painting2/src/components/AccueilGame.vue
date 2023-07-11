<template>
   <div id="Accueil">
      <h1>Accueil et choix du jeu</h1>
      <div id="gamemod">
         <div class="GameCard">
            <a href="#/GuessThePainting">
               <img :src="currentPainting ? getImagePath(currentPainting.img) : ''" alt="">
               <h2>Jeu n°1</h2>
            </a>
         </div>
         <div class="GameCard">
            <a href="#/">
               <img :src="currentPainting ? getImagePath(currentPainting.img) : ''" alt="">
               <h2>Jeu n°X (test card)</h2>
            </a>
         </div>
      </div>
   </div>
</template>
<script setup>
import paintingsData from "@/data/paintings.json";
import { ref, onMounted, watch } from "vue";

const paintings = ref([]);
const currentPainting = ref(null);

onMounted(() => {
   loadPaintings();
});

const loadPaintings = () => {
   paintings.value = paintingsData.tableaux;
   nextPainting();
};

const nextPainting = () => {
   currentPainting.value = getRandomPainting();
};

const getRandomPainting = () => {
   let randomIndex = Math.floor(Math.random() * paintings.value.length);
   let randomPainting = paintings.value[randomIndex];

   while (randomPainting === currentPainting.value) {
      randomIndex = Math.floor(Math.random() * paintings.value.length);
      randomPainting = paintings.value[randomIndex];
   }

   return randomPainting;
};

const getImagePath = (imageName) => {
   return `/assets/${imageName}`;
};

watch(currentPainting, (newPainting) => {
   console.log("New painting", newPainting);
});
</script>
<style lang="scss">
#gamemod {
   display: flex;
   flex-wrap: wrap;
   justify-content: space-around;
}

.GameCard {
   display: flex;
   height: 40vh;
   width: 30vw;
   min-width: 250px;
   align-items: center;
   justify-content: center;
   flex-direction: column;
   border-radius: 10px;
   transition: 0.3s ease-in-out;
   z-index: 5;
   margin-bottom: 70px;

   &:hover {
      cursor: pointer;
      transform: scale(1.04);
   }
}

.GameCard a,
.GameCard img {
   height: 100%;
   width: 100%;
}

.GameCard img {
   object-fit: cover;
   border-radius: 10px;
}

.GameCard a {
   text-decoration: none;
}
</style>