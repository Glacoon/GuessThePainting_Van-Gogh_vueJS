<template>
  <div class="guess-the-painting">
    <p class="score">Score: {{ score }} - Highest score: {{ highestScore }}</p>
    <transition name="slide-in" mode="out-in">
      <div class="painting-container" v-if="currentPainting" :key="currentPainting">
        <img :src="getImagePath(currentPainting.img)" alt="Painting" v-if="showImage" />
      </div>
    </transition>
    <div class="options">
      <button v-for="option in options" :key="option" @click="checkAnswer(option, $event.target)" :class="option">
        {{ option }}
      </button>
    </div>
    <div class="showResult" v-if="showResult">
      <p v-if="isCorrectAnswer">Correct!</p>
      <p v-else>Wrong answer. Try again!</p>
    </div>
  </div>
</template>

<script setup>
import paintingsData from "@/data/paintings.json";
import { ref, onMounted, watch } from "vue";

const paintings = ref([]);
const currentPainting = ref(null);
const options = ref([]);
const showResult = ref(false);
const isCorrectAnswer = ref(false);
const showImage = ref(false);
const score = ref(0);
const highestScore = ref(0);

function incrementScore() {
  score.value++;
  if (score.value > highestScore.value) {
    highestScore.value = score.value;
  }
}

function desactivateAllButtons() {
  const buttons = document.querySelectorAll("button");
  buttons.forEach((button) => {
    button.disabled = true;
  });
}

function resetStyleButtons() {
  const buttons = document.querySelectorAll("button");
  buttons.forEach((button) => {
    button.style.border = "none";
  });
}

onMounted(() => {
  loadPaintings();
});

const loadPaintings = () => {
  paintings.value = paintingsData.tableaux;
  nextPainting();
};

const nextPainting = () => {
  showResult.value = false;
  isCorrectAnswer.value = false;
  currentPainting.value = getRandomPainting();
  options.value = getOptions();
  animerCurrentPainting();
};

const getRandomPainting = () => {
  let randomPainting = paintings.value[Math.floor(Math.random() * paintings.value.length)];
  while (randomPainting === currentPainting.value) {
    randomPainting = paintings.value[Math.floor(Math.random() * paintings.value.length)];
  }
  return randomPainting;
};

const getOptions = () => {
  const options = [];
  options.push(currentPainting.value.titleFR);
  while (options.length < 4) {
    const randomPainting = getRandomPainting();
    if (!options.includes(randomPainting.titleFR)) {
      options.push(randomPainting.titleFR);
    }
  }
  return options.sort(() => Math.random() - 0.5);
};

const checkAnswer = (option, button) => {
  showResult.value = true;
  if (option === currentPainting.value.titleFR) {
    button.style.border = "5px solid green";
    desactivateAllButtons();
    incrementScore();
    isCorrectAnswer.value = true;
    setTimeout(() => {
      nextPainting();
      resetStyleButtons();
    }, 1000);
  } else {
    score.value = 0;
    isCorrectAnswer.value = false;
    button.style.border = "5px solid red";
  }
};

const animerCurrentPainting = () => {
  showImage.value = false;
  setTimeout(() => {
    showImage.value = true;
  }, 100);
};

const getImagePath = (imageName) => {
  return `/assets/${imageName}`;
};

watch(currentPainting, (newPainting) => {
  console.log("New painting", newPainting);
});

watch(isCorrectAnswer, (newAnswer) => {
  console.log("New answer", newAnswer);
});

watch(showResult, (newResult) => {
  console.log("New result", newResult);
});

watch(options, (newOptions) => {
  console.log("New options", newOptions);
});
</script>

<style scoped lang="scss">
/* Styles CSS du composant GuessThePainting */
</style>


<style scoped lang="scss">
.slide-in-enter-active {
  animation: slide-in 0.5s ease-in-out;
}

@keyframes slide-in {
  0% {
    transform: translateX(100%);
  }

  100% {
    transform: translateX(0);
  }
}

.slide-in-leave-active {
  animation: slide-out 0.5s ease-in-out;
}

@keyframes slide-out {
  0% {
    transform: translateX(0);
  }

  100% {
    transform: translateX(-100%);
  }
}

.guess-the-painting {
  text-align: center;
  margin-top: 20px;
}

.painting-container {
  margin-bottom: 20px;
  height: 55vmin;
}

.showResult p {
  font-size: 1.5em;
  font-weight: bold;
}

.painting-container img {
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  max-width: 100%;
  max-height: 100%;
}

.options {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  gap: 10px;
  margin-bottom: 20px;
  padding: 0 10vw 0 10vw;
}

.options button {
  width: 300px;
  height: 3.5em;
  font-size: 1.2em;
  border: none;
  border-radius: 50px;
  background-color: #4b627a;
  color: #fff;
  cursor: pointer;
  transition: 0.2s ease-in-out;
  &:hover {
    background-color: #fff;
    color: #4b627a;
  }
}
</style>
