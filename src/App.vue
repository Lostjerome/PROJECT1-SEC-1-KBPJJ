<script setup>
import { ref, computed } from "vue";
import moon from "./components/icons/moon.vue";
import sun from "./components/icons/sun.vue";

const showHowtToPlay = ref(false);

const questions = ref([]);
const categories = ref();
const score = ref(0);
const currentQuestion = ref(0);
const inputMusic = ref(null);

const nightMode = ref(false);
const isMusicPlay = ref(false);

const selectedCategory = ref("");
const selectedDifficulty = ref("");

const isPlaying = ref(false);

const difficulties = [
  {
    title: "easy",
    description: "A piece of cake.",
    bg: "bg-green-500",
    selectedbg: "bg-green-600 border-black",
  },
  {
    title: "medium",
    description: "Time to put your skill to the test!",
    bg: "bg-amber-300",
    selectedbg: "bg-amber-400 border-black",
  },
  {
    title: "hard",
    description: `Your're smart? Prove it with this one!`,
    bg: "bg-red-500",
    selectedbg: " border border-solid bg-red-600 border-black",
  },
];

fetch("https://the-trivia-api.com/api/categories")
  .then((response) => response.json())
  .then((data) => (categories.value = data));

function getQuestions(category, difficulty) {
  fetch(
    `https://the-trivia-api.com/api/questions?categories=${category}&limit=10&region=TH&difficulty=${difficulty}`
  )
    .then((response) => response.json())
    .then((data) => (questions.value = data))
    .catch((error) => console.log(error));

  console.log(questions.value);
  return questions.value;
}
getQuestions("film", "easy");

function getChoice(choiceTarget) {
  return shuffle([
    choiceTarget?.correctAnswer,
    ...choiceTarget?.incorrectAnswers,
  ]);
}

function shuffle(array) {
  return array?.sort(() => Math.random() - 0.5);
}

const playAndPause = () => {
  isMusicPlay.value = !isMusicPlay.value;
  // if (isMusicPlay.value) inputMusic.value.play();
  // else inputMusic.value.pause();
};

const darkModeSet = (item) => {
  if (nightMode.value === true) {
    if (item === "block") {
      return "bg-[#242731] text-white hover:bg-slate-700";
    } else if (item === "font") {
      return "text-white";
    } else if (item === "border") {
      return "border-white text-white";
    } else if (item === "bg") {
      return "bg-[#1F2128]";
    } else if (item === "btn") {
      return "bg-teal-500 text-white hover:bg-slate-600 hover:text-white";
    }
  }
};
</script>

<template>
  <div>
    <div v-show="!isPlaying">
      <div :class="nightMode ? darkModeSet('bg') : ''" class="h-full">
        <!-- night mode btn -->
        <div class="fixed top-12 right-1 md:right-8">
          <label
            class="swap swap-rotate neutral justify-center mr-6 w-10 h-10 rounded-full shadow-lg"
            :class="nightMode ? darkModeSet('block') : 'bg-slate-200'"
          >
            <input type="checkbox" v-model="nightMode" />
            <moon class="swap-on w-5 h-5" />
            <sun class="swap-off w-5 h-5" />
          </label>
        </div>

        <!-- header -->
        <div class="w-full h-screen flex items-center justify-center" id="head">
          <div class="md:grid md:grid-cols-2 content-center w-full">
            <div
              class="flex flex-col justify-center md:ml-24 text-center md:text-left"
            >
              <h1
                :class="nightMode ? darkModeSet('font') : 'text-black'"
                class="text-3xl md:text-left flex justify-center md:mr-6 font-bold font-mono md:text-6xl"
              >
                Quiz ja pak quiz tueng quiz kat
              </h1>
              <p
                class="md:text-left mt-8"
                :class="nightMode ? darkModeSet('font') : 'text-black'"
              >
                A quiz IQ with this quiz
              </p>

              <div class="flex flex-row mt-5 md:justify-start justify-center">
                <a href="#categories">
                  <button
                    :class="nightMode ? darkModeSet('font') : 'text-black'"
                    class="bg-teal-500 p-2 pl-4 pr-4 mt-4 rounded-lg hover:bg-black hover:text-white shadow-lg"
                    @click="
                      () => {
                        selectedCategory = categories[i].reduce((a, b) =>
                          a.length > b.length ? a : b
                        );
                      }
                    "
                  >
                    get start
                  </button>
                </a>
                <button
                  :class="
                    nightMode
                      ? darkModeSet('border')
                      : 'border-black text-black'
                  "
                  class="bg-transparent border p-2 pl-4 pr-4 mt-4 rounded-lg hover:bg-black hover:text-white ml-3 shadow-lg"
                  @click="showHowtToPlay = !showHowtToPlay"
                >
                  how to play
                </button>
              </div>
            </div>

            <div class="w-11/12 hidden md:block">
              <img src="./assets/image/cover.png" alt="quiz" class="" />
            </div>
          </div>
        </div>
      </div>
      <!-- CATEGORIES -->
      <div id="categories" class="md:pt-3 py-7 px-10 text-center">
        <div class="md:text-4xl text-2xl md:none font-extrabold md:my-12 my-6">
          <h1 class="text-4xl font-extrabold">
            What
            <span
              class="bg-gradient-to-r from-green-400 to-blue-500 bg-clip-text text-transparent"
              >category</span
            >
            of quiz would you like to take?
          </h1>
        </div>
        <div class="grid md:grid-cols-4 grid-cols-2 gap-5 md:px-48 font-medium">
          <!-- for...in ; got property of object -->
          <a
            href="#level"
            v-for="(prop, key) in categories"
            :key="key"
            class="py-5 p-3 text-center border rounded-lg shadow-lg hover:bg-gray-400 duration-200"
            @click="key = prop[0]"
            :class="
              key === prop[0]
                ? 'border-gray-200 bg-gray-200 shadow-black'
                : 'bg-gray-200'
            "
          >
            {{ key }}
          </a>
        </div>
      </div>

      <!-- LEVEL -->
      <div id="level" class="md:py-6 py-10 px-9 text-center">
        <div class="md:text-4xl text-2xl font-extrabold md:my-14 my-7">
          <h1 class="text-4xl font-extrabold">
            What
            <span
              class="bg-gradient-to-r from-green-400 to-blue-500 bg-clip-text text-transparent"
              >difficulty</span
            >
            would you like to start with?
          </h1>
        </div>

        <div
          class="grid md:grid-cols-3 grid-cols-1 gap-7 md:px-32 px-5 font-medium"
        >
          <button
            id="easy"
            class="h-96 border rounded-3xl px-9 pt-64 text-start"
            v-for="(level, key) in difficulties"
            :key="key"
            @click="selectedDifficulty = level.title"
            :class="
              selectedDifficulty === level.title ? level.selectedbg : level.bg
            "
          >
            <p class="text-3xl font-extrabold">
              {{ level.title.toLocaleUpperCase() }}
            </p>
            <p class="">{{ level.description }}</p>
          </button>
        </div>

        <div
          class="flex md:justify-between justify-end mt-10 md:px-32 md:none m-5"
        >
          <a href="#test">
            <button
              class="border rounded-lg py-1 px-5 bg-blue-500 text-white font-bold tracking-wide shadow-lg"
              @click="
                () => {
                  getQuestions(selectedCategory, selectedDifficulty);
                  isPlaying = !isPlaying;
                  playAndPause();
                }
              "
            >
              Lets Play
            </button></a
          >
        </div>
      </div>
    </div>
    <div v-show="isPlaying">
      <div
        class="min-h-screen min-w-full flex flex-col items-center bg-gray-400"
      >
        <!-- nav-Bar -->
        <div
          class="flex flex-row bg-slate-200 w-5/6 h-10 mt-6 items-center justify-between rounded-full"
        >
          <!-- button-back -->
          <button
            class="border-solid border h-8 w-16 bg-white rounded-full ml-2 hover:bg-slate-400"
            @click="isPlaying = !isPlaying"
          >
            &#60;&nbsp;Back
          </button>

          <!-- score -->
          <p
            class="border-solid border h-8 w-24 mr-8 text-center bg-green-400 rounded-full"
          >
            Score:<span>&nbsp;{{ score }}</span>
          </p>

          <!-- dark-mode -->
          <button
            class="border-solid border h-9 w-9 bg-white rounded-full mr-2"
          >
            DM
          </button>
        </div>

        <!-- content -->
        <div class="grid grid-col-1 w-4/6">
          <!-- Question -->
          <div
            class="flex flex-col bg-white w-full h-auto mt-10 items-center justify-center rounded-3xl py-7 px-3"
          >
            <div class="text-center">
              <p class="mt-3 text-gray-800">
                Question {{ currentQuestion + 1 }}/10
              </p>
              <h2 class="font-semibold break-all m-3 text-4xl items-center">
                {{ questions[currentQuestion]?.question }}
              </h2>
            </div>
          </div>

          <!-- Choice -->
          <div
            class="grid grid-cols-1 gap-3 bg-slate-300 mt-5 py-5 px-10 rounded-3xl w-full lg:grid-cols-2"
          >
            <div
              v-for="(choice, index) in getChoice(questions[currentQuestion])"
              :key="index"
              id="getC"
              @click="
                userClick = choice;
                userClick === questions[currentQuestion]?.correctAnswer
                  ? score++
                  : (score += 0);
                currentQuestion == questions.length - 1
                  ? (currentQuestion += 0)
                  : currentQuestion++;
                countUserClick++;
                countUserClick > questions.length - 1
                  ? (checkFinishTest = true)
                  : (checkFinishTest = false);
              "
            >
              <button
                class="bg-white rounded-3xl break-all py-3 px-2 w-full lg:px-10 hover:bg-slate-400"
              >
                {{ choice }}
              </button>
            </div>
          </div>

          <!-- reload-Button  -->
          <div class="flex mt-7 justify-center">
            <button
              class="h-9 w-9 bg-white rounded-full mr-2 hover:bg-slate-400"
              @click="
                score = 0;
                currentQuestion = 0;
                checkFinishTest = false;
                countUserClick = 0;
                getQuestions();
              "
            >
              RL
            </button>
          </div>

          <!-- REF -->
          <div
            class="flex bg-gray-600 rounded-3xl w-full justify-center p-4 mt-10"
          >
            <p>
              All questions are from
              <a
                class="hover:text-orange-100"
                href="https://the-trivia-api.com/docs/#search"
                >The Trivia API</a
              >
            </p>
          </div>
        </div>

        <!-- result-score -->
        <div
          class="bg-fixed absolute min-w-full min-h-screen flex flex-col items-center"
          v-show="checkFinishTest"
        >
          <div
            class="flex flex-col w-3/6 h-full bg-white text-center mt-40 rounded-3xl p-2 items-center"
          >
            <p class="m-3 break-all">
              You've finished all quizes. Here's your score
            </p>

            <!-- circle-score -->
            <div class="bg-gray-300 rounded-full w-36 h-36 pt-9 mt-10">
              <p class="text-4xl font-extrabold">{{ score }}</p>
              <p class="font-medium">Out of 10</p>
            </div>

            <!-- button-action -->
            <div
              class="flex flex-col gap-4 font-bold mt-5 py-4 w-3/4 text-center"
            >
              <!--button-play-again -->
              <button
                class="bg-blue-500 text-white rounded-full w-full py-2 hover:bg-blue-600"
                @click="
                  score = 0;
                  currentQuestion = 0;
                  checkFinishTest = false;
                  countUserClick = 0;
                  getQuestions();
                "
              >
                Play again
              </button>
              <!--button-home -->
              <button
                class="bg-gray-100 rounded-full p-2 py-2 hover:bg-gray-200"
              >
                Home
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
