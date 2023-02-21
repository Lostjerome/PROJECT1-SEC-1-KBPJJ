<script setup>
import { ref, computed, watch } from "vue";

import moon from "./components/icons/moon.vue";
import sun from "./components/icons/sun.vue";
import reload from "./components/icons/reload.vue";
import volumeOn from "./components/icons/volume-on.vue";
import volumeOff from "./components/icons/volume-off.vue";
import back from "./components/icons/arrow-left.vue";

import themeSong from "./assets/audio/gameShow.mp3";

import difficulties from "./constant/difficulties";

// data
const questions = ref([]);
const categories = ref([]);
const selectedCategory = ref("");
const selectedDifficulty = ref("");
const score = ref(0);
const currentQuestionNumber = ref(0);
const inputMusicRef = ref(null);

// theme
const themeDark = ref({
  "background-color": "#1F2128",
  color: "#FFFFFF",
});
const themeLight = ref({
  "background-color": "#f2f2f2",
  color: "#000000",
});
const buttonDark = ref("bg-[#4a5066] md:hover:bg-[#3d4253] border-white ");
const buttonLight = ref("bg-white md:hover:bg-slate-100 border-black ");

// status
const darkMode = ref(false);
const isMusicPlaying = ref(false);
const isPlaying = ref(false);
const isFinish = ref(false);
const showHowToPlay = ref(false);
const showAnswer = ref(false);

// ref
const categoriesRef = ref(null);
const difficultiesRef = ref(null);
const playButtonRef = ref(null);

const backToHome = () => {
  isPlaying.value = false;
  isFinish.value = false;
  currentQuestionNumber.value = 0;
  score.value = 0;
  playAndPause("stop");
  setTimeout(() => {
    categoriesRef.value.scrollIntoView({ behavior: "smooth" });
  }, 1);
};

const restartPlaying = () => {
  getQuestions(selectedCategory.value, selectedDifficulty.value).then(() => {
    isFinish.value = false;
    currentQuestionNumber.value = 0;
    score.value = 0;
  });
};

const getCategories = async () => {
  const response = await fetch(
    "https://the-trivia-api.com/api/categories?region=TH"
  );
  const jsonData = await response.json();
  categories.value = jsonData;
};

const getQuestions = async (category, difficulty) => {
  const response = await fetch(
    `https://the-trivia-api.com/api/questions?categories=${category}&limit=10&region=TH&difficulty=${difficulty}`
  );
  const jsonData = await response.json();
  questions.value = jsonData;
};

// function getChoice(currentQuestion) {
//   return shuffle([
//     currentQuestion?.correctAnswer,
//     ...currentQuestion?.incorrectAnswers,
//   ]);
// }

const choices = computed(() => {
  let choices = [];
  if (questions.value[currentQuestionNumber.value]) {
    choices = [
      ...questions.value[currentQuestionNumber.value].incorrectAnswers,
      questions.value[currentQuestionNumber.value].correctAnswer,
    ];
    shuffle(choices);
  }
  return choices;
});

const toggleDarkMode = () => {
  darkMode.value = !darkMode.value;
};

const ansOnClick = (ans) => {
  showAnswer.value = true;
  ans === questions.value[currentQuestionNumber.value]?.correctAnswer &&
    score.value++;

  setTimeout(() => {
    currentQuestionNumber.value == questions.value.length - 1
      ? (isFinish.value = true)
      : currentQuestionNumber.value++;
    showAnswer.value = false;
  }, 1000);
};

const startPlaying = () => {
  getQuestions(selectedCategory.value, selectedDifficulty.value).then(() => {
    isPlaying.value = !isPlaying.value;
    setTimeout(() => {
      window.scrollTo(0, 0);
    }, 100);
    playAndPause("restart");
  });
};

const shuffle = (array) => {
  return array?.sort(() => Math.random() - 0.5);
};

const playAndPause = (param) => {
  inputMusicRef.value.volume = 0.05;

  if (param === "stop") {
    isMusicPlaying.value = false;
    inputMusicRef.value.pause();
    return;
  }
  if (param === "restart") {
    isMusicPlaying.value = true;
    //restart music
    inputMusicRef.value.currentTime = 0;
    inputMusicRef.value.play();
    return;
  }
  isMusicPlaying.value = !isMusicPlaying.value;
  if (isMusicPlaying.value) {
    inputMusicRef.value.play();
    //decrease volume
  } else inputMusicRef.value.pause();
};

watch(
  () => selectedCategory.value && selectedDifficulty.value,
  () => {
    setTimeout(() => {
      playButtonRef.value.scrollIntoView({ behavior: "smooth" });
    }, 1);
  }
);

// on setup
getCategories();
</script>

<template>
  <div>
    <div v-show="!isPlaying">
      <div :style="darkMode ? themeDark : themeLight" class="h-full">
        <!-- night mode btn -->
        <button
          @click="toggleDarkMode"
          :class="darkMode ? buttonDark + 'fill-white' : buttonLight"
          class="fixed top-5 right-5 p-2 z-20 drop-shadow-md border rounded-full duration-100"
        >
          <sun class="w-6 h-6" v-show="!darkMode" />
          <moon class="w-6 h-6" v-show="darkMode" />
        </button>

        <!-- header -->
        <div class="w-full h-screen flex items-center justify-center">
          <div class="md:grid md:grid-cols-2 content-center w-full">
            <div
              class="flex flex-col justify-center md:ml-24 text-center md:text-left"
            >
              <h1
                class="text-3xl md:text-6xl text-center md:text-left md:mr-6 font-bold"
              >
                Become a quiz master and impress your friends
              </h1>
              <p class="md:text-left mt-8 text-xl">
                Where guessing is always an option
              </p>

              <div class="flex flex-row mt-5 md:justify-start justify-center">
                <button
                  @click="
                    () => {
                      categoriesRef.scrollIntoView({ behavior: 'smooth' });
                    }
                  "
                  :class="darkMode ? 'border-white' : 'border-black'"
                  class="bg-teal-500 text-white font-bold p-2 pl-4 pr-4 mt-4 rounded-lg border md:hover:bg-black md:hover:text-white shadow-lg duration-200"
                >
                  Get started
                </button>
                <button
                  :class="darkMode ? 'border-white' : 'border-black '"
                  class="border p-2 pl-4 pr-4 mt-4 rounded-lg md:hover:bg-black md:hover:text-white ml-3 shadow-lg duration-200"
                  @click="showHowToPlay = !showHowToPlay"
                >
                  How to play
                </button>
              </div>
            </div>

            <div class="w-11/12 hidden md:block">
              <img src="./assets/image/cover.png" alt="quiz" class="" />
            </div>
          </div>
        </div>

        <div class="md:max-w-[60rem] m-auto px-5">
          <!-- CATEGORIES -->
          <div ref="categoriesRef" class="md:pt-3 py-7 text-center">
            <div class="md:text-4xl text-2xl font-extrabold my-12">
              <h1 class="text-4xl font-extrabold">
                What
                <span
                  class="bg-gradient-to-r from-green-400 to-blue-500 bg-clip-text text-transparent"
                  >category</span
                >
                of quiz would you like to take?
              </h1>
            </div>
            <div
              class="grid md:grid-cols-4 grid-cols-2 gap-3 m-auto font-medium"
            >
              <!-- for...in ; got property of object -->
              <button
                v-for="(category, key) in [...Object.keys(categories)]"
                :key="key"
                :class="
                  categories[category].includes(selectedCategory)
                    ? 'bg-teal-500 text-white border-black'
                    : darkMode
                    ? buttonDark
                    : buttonLight
                "
                class="py-5 px-3 text-center rounded-xl font-bold drop-shadow-lg duration-200 border"
                @click="
                  () => {
                    selectedCategory = categories[category].reduce((a, b) =>
                      a.length > b.length ? a : b
                    );
                    difficultiesRef.scrollIntoView({ behavior: 'smooth' });
                  }
                "
              >
                {{ category }}
              </button>
            </div>
          </div>

          <!-- DIFFICULTIES -->
          <div ref="difficultiesRef" class="md:py-6 py-10 text-center">
            <div class="md:text-4xl text-2xl font-extrabold md:my-14 my-7">
              <h1 class="text-4xl font-black">
                What
                <span
                  class="bg-gradient-to-r from-green-400 to-blue-500 bg-clip-text text-transparent"
                  >difficulty</span
                >
                would you like to start with?
              </h1>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-3 gap-3 m-auto">
              <button
                v-for="(difficulty, key) in difficulties"
                :key="key"
                @click="
                  () => {
                    selectedDifficulty = difficulty.title;
                  }
                "
                :class="
                  selectedDifficulty
                    ? selectedDifficulty === difficulty.title
                      ? difficulty.selectedbg
                      : darkMode
                      ? buttonDark
                      : 'bg-gray-300 md:hover:bg-gray-400 border-black'
                    : difficulty.bg
                "
                class="rounded-3xl border-black border px-5 pt-20 md:pt-52 pb-10 text-start duration-200 drop-shadow-lg"
              >
                <p class="text-3xl font-bold">
                  {{ difficulty.title.toLocaleUpperCase() }}
                </p>
                <p>{{ difficulty.description }}</p>
              </button>
            </div>
            <div class="flex justify-between">
              <div></div>
              <button
                ref="playButtonRef"
                v-show="selectedCategory && selectedDifficulty"
                :class="darkMode ? 'border-white' : 'border-black'"
                class="mt-10 rounded-lg py-2 px-7 bg-teal-500 md:hover:bg-black duration-200 text-white font-bold tracking-wide shadow-lg border"
                @click="startPlaying"
              >
                Let's Play
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div :style="darkMode ? themeDark : ''" v-show="isPlaying">
      <div
        :class="darkMode ? 'bg-[#1F2128]' : 'bg-gray-400'"
        class="min-h-screen min-w-full flex flex-col items-center"
      >
        <!-- nav-Bar -->
        <div
          :class="
            darkMode ? 'bg-[#3d4253] border-white' : 'bg-white border-black'
          "
          class="p-1 md:p-2 flex flex-row fixed z-10 border drop-shadow-lg w-[calc(100%-2.5rem)] max-w-[64rem] mt-6 items-center justify-between rounded-full"
        >
          <!-- button-back -->
          <button
            :class="darkMode ? buttonDark + 'fill-white' : buttonLight"
            class="border-solid border flex p-1 md:p-2 px-2 md:px-4 rounded-full duration-100"
            @click="backToHome"
          >
            <back class="w-6 h-6" />Back
          </button>

          <!-- score -->
          <p
            :class="darkMode ? 'bg-emerald-500' : 'bg-teal-400'"
            class="p-1 md:p-2 px-4 md:px-6 text-center rounded-full font-bold"
          >
            Score:<span>&nbsp;{{ score }}</span>
          </p>

          <!-- dark-mode -->
          <div class="space-x-2 flex">
            <button
              :class="darkMode ? buttonDark + 'fill-white' : buttonLight"
              class="p-2 border rounded-full duration-100"
              @click="playAndPause"
            >
              <audio ref="inputMusicRef" :src="themeSong" loop></audio>
              <volumeOn class="w-5 h-5 md:w-6 md:h-6" v-show="isMusicPlaying" />
              <volumeOff
                class="w-5 h-5 md:w-6 md:h-6"
                v-show="!isMusicPlaying"
              />
            </button>
            <button
              @click="toggleDarkMode"
              :class="darkMode ? buttonDark + 'fill-white' : buttonLight"
              class="p-2 border rounded-full duration-100"
            >
              <sun class="w-5 h-5 md:w-6 md:h-6" v-show="!darkMode" />
              <moon class="w-5 h-5 md:w-6 md:h-6" v-show="darkMode" />
            </button>
          </div>
        </div>

        <!-- content -->
        <div
          class="w-full p-5 md:max-w-[48rem] absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2"
        >
          <!-- Question -->
          <div
            :class="
              darkMode ? 'bg-[#4a5066] border-white' : 'bg-white border-black'
            "
            class="flex flex-col border h-auto mt-10 items-center justify-center rounded-2xl md:rounded-3xl py-3 md:py-5 md:px-2"
          >
            <div class="text-center">
              <p
                :class="darkMode ? 'text-white' : 'ext-gray-800'"
                class="mt-3 text-sm md:text-base"
              >
                Question {{ currentQuestionNumber + 1 }}/{{ questions.length }}
              </p>
              <h2 class="font-bold m-3 text-lg md:text-2xl items-center">
                {{ questions[currentQuestionNumber]?.question }}
              </h2>
            </div>
          </div>

          <!-- Choice -->
          <div
            :class="
              darkMode
                ? 'bg-[#3d4253] border-white'
                : 'bg-slate-300 border-black '
            "
            class="grid grid-cols-1 gap-3 border mt-5 p-3 md:p-5 rounded-2xl md:rounded-3xl md:grid-cols-2"
          >
            <button
              v-for="(choice, key) in choices"
              :key="key"
              @click="!showAnswer && ansOnClick(choice)"
              :class="
                showAnswer
                  ? questions[currentQuestionNumber]?.correctAnswer === choice
                    ? 'bg-green-400 border-green-700 text-black'
                    : darkMode
                    ? buttonDark
                    : buttonLight
                  : darkMode
                  ? buttonDark
                  : buttonLight
              "
              class="rounded-lg md:rounded-xl border py-3 md:py-4 px-2 w-full duration-100"
            >
              {{ choice }}
            </button>
          </div>

          <!-- reload-Button  -->
          <div class="flex mt-7 justify-center">
            <button
              :class="
                darkMode
                  ? buttonDark + 'fill-white'
                  : 'bg-white border-black md:hover:bg-slate-100'
              "
              class="p-2 border rounded-full mr-2 duration-100"
              @click="restartPlaying"
            >
              <reload class="w-6 h-6 md:w-8 md:h-8" />
            </button>
          </div>
        </div>
        <!-- REF -->
        <div
          :class="darkMode ? 'bg-[#3d4253]' : 'bg-slate-300 border-black'"
          class="border rounded-lg md:rounded-2xl w-[calc(100%-2.5rem)] max-w-[52rem] p-2 py-4 md:p-4 md:py-7 mt-10 text-center fixed left-1/2 -translate-x-1/2 bottom-5"
        >
          <p>
            All questions are from
            <a
              target="_blank"
              :class="
                darkMode
                  ? 'text-purple-300 md:hover:text-purple-400'
                  : 'text-purple-600 md:hover:text-purple-700'
              "
              class="font-bold"
              href="https://the-trivia-api.com"
              >The Trivia API</a
            >
          </p>
        </div>
        <!-- result-score -->
        <div
          v-show="isFinish"
          class="w-screen h-full absolute bg-[#0000007a] bottom-0 backdrop-blur-sm z-20"
        ></div>
        <div
          :class="darkMode ? 'bg-[#4a5066]' : 'bg-white border-black'"
          class="flex flex-col z-20 w-[calc(100%-2.5rem)] max-w-[32em] border text-center rounded-3xl p-8 py-5 items-center absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2"
          v-show="isFinish"
        >
          <p class="font-bold text-2xl">Hooray!</p>
          <p class="m-3">
            You've finished all {{ selectedCategory }} quizes with
            {{ selectedDifficulty }} level. <br />
            Here's your score
          </p>

          <!-- circle-score -->
          <div
            :class="darkMode ? 'bg-[#3d4253]' : 'bg-gray-300 border-black'"
            class="border rounded-full w-36 h-36 pt-9 mt-10"
          >
            <p class="text-4xl font-extrabold">{{ score }}</p>
            <p class="font-medium">Out of {{ questions.length }}</p>
          </div>

          <!-- button-action -->
          <div
            class="flex flex-col md:flex-row-reverse gap-2 font-bold mt-5 py-4 w-full text-center"
          >
            <!--button-play-again -->
            <button
              class="bg-teal-500 text-white rounded-lg w-full py-2 md:hover:bg-teal-600 duration-200"
              @click="restartPlaying"
            >
              Play again
            </button>
            <!--button-home -->
            <button
              @click="backToHome"
              :class="darkMode ? buttonDark : buttonLight"
              class="border rounded-lg w-full p-2 py-2 duration-200"
            >
              Try other one
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
