<script>

import HeadGuess from "@/components/HeadGuess.vue";
import GuessThePainting from "@/components/GuessThePainting.vue";
import Accueil from "@/components/AccueilGame.vue";

import { ref, computed } from "vue";

export default {
  name: 'App',
  components: {
    HeadGuess,
    GuessThePainting,
    Accueil
  },
  setup() {
    const routes = {
      '/GuessThePainting':GuessThePainting,
      '/':Accueil
    };

    const currentRoute = ref(window.location.hash);

    window.addEventListener('hashchange', () => {
      currentRoute.value = window.location.hash;
    });

    const currentView = computed(() => {
      return routes[currentRoute.value.slice(1) || '/'];
    })

    return {
      currentView
    }
  }
}
</script>
<template>
  <div id="app">
    <HeadGuess />
    <component :is="currentView" />
  </div>
</template>
<style>
body{
  background-color: #161e25 !important;
  padding: 0 15vh 0 15vh;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #4b627a;
  margin-top: 40px;
}
</style>
