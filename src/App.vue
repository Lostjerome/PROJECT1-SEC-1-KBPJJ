<script setup>
import { ref, computed } from "vue";
import moon from "./components/icons/moon.vue";
import sun from "./components/icons/sun.vue";

const showHowtToPlay = ref(false);

const question = ref([]);
const categories = ref();

const nightMode = ref(false);

const category = ref("");
const difficulty = ref("");

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

function getQuestion(category, difficulty) {
  fetch(
    `https://the-trivia-api.com/api/questions?categories=${category}&limit=10&region=TH&difficulty=${difficulty}`
  )
    .then((response) => response.json())
    .then((data) => (question.value = data))
    .catch((error) => console.log(error));

  return randomQuestion(question.value);
}

function randomQuestion(questions) {
  const random = Math.floor(Math.random() * 10);
  return questions[random];
}

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
    <div v-show="isPlaying">
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
            @click="difficulty = level.title"
            :class="difficulty === level.title ? level.selectedbg : level.bg"
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
          <a
            href="#test"
            @click="
              getQuestions(category, difficulty);
              letsPlay = !letsPlay;
              playAndPause();
            "
          >
            <button
              class="border rounded-lg py-1 px-5 bg-blue-500 text-white font-bold tracking-wide shadow-lg"
              @click="
                () => {
                  getQuestions(category, difficulty);
                  letsPlay = !letsPlay;
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
    <div v-show="!isPlaying"></div>
  </div>
</template>

<style scoped></style>
