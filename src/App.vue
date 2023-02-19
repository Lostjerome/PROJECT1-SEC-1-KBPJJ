<script setup>
import { ref, computed } from "vue";
import moon from "./components/icons/moon.vue";
import sun from "./components/icons/sun.vue";

const showHowtToPlay = ref(false);

const questions = ref([]);
const categories = ref();
const score = ref(0);
const currentQuestionNumber = ref(0);
const inputMusic = ref(null);

const nightMode = ref(false);
const isMusicPlay = ref(false);
const categoriesSection = ref(null);
const difficultiesSection = ref(null);

const selectedCategory = ref("");
const selectedDifficulty = ref("");

const isPlaying = ref(false);
const isFinish = ref(false);

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
getCategories();
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
                <button
                  @click="
                    () => {
                      categoriesSection.scrollIntoView({ behavior: 'smooth' });
                    }
                  "
                  :class="nightMode ? darkModeSet('font') : 'text-black'"
                  class="bg-teal-500 p-2 pl-4 pr-4 mt-4 rounded-lg hover:bg-black hover:text-white shadow-lg"
                >
                  Get started
                </button>
                <button
                  :class="
                    nightMode
                      ? darkModeSet('border')
                      : 'border-black text-black'
                  "
                  class="bg-transparent border p-2 pl-4 pr-4 mt-4 rounded-lg hover:bg-black hover:text-white ml-3 shadow-lg"
                  @click="showHowtToPlay = !showHowtToPlay"
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
      </div>
      <!-- CATEGORIES -->
      <div class="md:w-[52rem] m-auto">
        <div ref="categoriesSection" class="md:pt-3 py-7 text-center">
          <div class="md:text-4xl text-2xl font-extrabold md:my-12 my-6">
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
            class="grid md:grid-cols-4 grid-cols-2 gap-3 md:w-[52rem] m-auto font-medium"
          >
            <!-- for...in ; got property of object -->
            <button
              v-for="(category, key) in [...Object.keys(categories)]"
              :key="key"
              :class="
                categories[category].includes(selectedCategory)
                  ? 'bg-gradient-to-r from-green-400 to-blue-500  text-white font-bold'
                  : 'bg-white hover:bg-gray-100 text-black '
              "
              class="py-5 text-center border rounded-xl drop-shadow-md duration-200"
              @click="
                () => {
                  selectedCategory = categories[category].reduce((a, b) =>
                    a.length > b.length ? a : b
                  );
                  difficultiesSection.scrollIntoView({ behavior: 'smooth' });
                }
              "
            >
              {{ category }}
            </button>
          </div>
        </div>

        <!-- DIFFICULTIES -->
        <div ref="difficultiesSection" class="md:py-6 py-10 text-center">
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
            class="grid md:grid-cols-3 grid-cols-1 gap-3 md:w-[52rem] m-auto"
          >
            <button
              v-for="(difficulty, key) in difficulties"
              :key="key"
              @click="selectedDifficulty = difficulty.title"
              :class="
                selectedDifficulty
                  ? selectedDifficulty === difficulty.title
                    ? difficulty.selectedbg
                    : 'bg-gray-300'
                  : difficulty.bg
              "
              class="border rounded-3xl px-5 pt-52 pb-10 text-start duration-200"
            >
              <p class="text-3xl font-extrabold">
                {{ difficulty.title.toLocaleUpperCase() }}
              </p>
              <p class="">{{ difficulty.description }}</p>
            </button>
          </div>

          <button
            v-show="selectedCategory && selectedDifficulty"
            class="mt-10 rounded-lg py-1 px-5 bg-blue-500 text-white font-bold tracking-wide shadow-lg"
            @click="
              () => {
                getQuestions(selectedCategory, selectedDifficulty).then(() => {
                  isPlaying = !isPlaying;
                  playAndPause();
                });
              }
            "
          >
            Lets Play
          </button>
        </div>
      </div>
    </div>
    <div v-show="isPlaying">
      <div
        class="min-h-screen min-w-full flex flex-col items-center bg-gray-400"
      >
        <!-- nav-Bar -->
        <div
          class="p-2 flex flex-row bg-slate-200 w-5/6 mt-6 items-center justify-between rounded-full"
        >
          <!-- button-back -->
          <button
            class="border-solid border h-8 w-16 bg-white rounded-full ml-2 hover:bg-slate-400"
            @click="
              () => {
                isPlaying = !isPlaying;
                playAndPause();
                selectedCategory = '';
                selectedDifficulty = '';
              }
            "
          >
            &#60;&nbsp;Back
          </button>

          <!-- score -->
          <p class="p-2 px-6 text-center bg-green-400 rounded-full">
            Score:<span>&nbsp;{{ score }}</span>
          </p>

          <!-- dark-mode -->
          <button
            class="border-solid border h-9 w-9 bg-white rounded-full mr-2"
          >
            DM
          </button>
        </div>
        <div
          class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2"
        >
          <!-- content -->
          <div class="grid grid-col-1 w-[48em]">
            <!-- Question -->
            <div
              class="flex flex-col bg-white h-auto mt-10 items-center justify-center rounded-3xl py-5 px-2"
            >
              <div class="text-center">
                <p class="mt-3 text-gray-800">
                  Question {{ currentQuestionNumber + 1 }}/{{
                    questions.length
                  }}
                </p>
                <h2 class="font-semibold break-all m-3 text-2xl items-center">
                  {{ questions[currentQuestionNumber]?.question }}
                </h2>
              </div>
            </div>

            <!-- Choice -->
            <div
              class="grid grid-cols-1 gap-3 bg-slate-300 mt-5 p-5 rounded-2xl w-full lg:grid-cols-2"
            >
              <button
                v-for="(choice, key) in choices"
                :key="key"
                @click="
                  choice === questions[currentQuestionNumber]?.correctAnswer &&
                    score++;
                  currentQuestionNumber == questions.length - 1
                    ? (isFinish = true)
                    : currentQuestionNumber++;
                "
                class="bg-white rounded-lg break-all py-3 w-full lg:px-10 hover:bg-slate-200 duration-200"
              >
                {{ choice }}
              </button>
            </div>

            <!-- reload-Button  -->
            <div class="flex mt-7 justify-center">
              <button
                class="h-9 w-9 bg-white rounded-full mr-2 hover:bg-slate-400"
                @click="
                  score = 0;
                  currentQuestionNumber = 0;
                  isFinish = false;
                  getQuestions(selectedCategory, selectedDifficulty);
                "
              >
                RL
              </button>
            </div>
          </div>
        </div>
        <!-- REF -->
        <div
          class="bg-gray-500 rounded-2xl w-[52rem] p-4 py-7 mt-10 text-center absolute left-1/2 -translate-x-1/2 bottom-5 text-white"
        >
          <p>
            All questions are from
            <a class="hover:text-orange-100" href="https://the-trivia-api.com"
              >The Trivia API</a
            >
          </p>
        </div>
        <!-- result-score -->
        <div
          v-show="isFinish"
          class="w-screen h-screen absolute bg-[#0000007a] bottom-0 backdrop-blur-sm"
        ></div>
        <div
          class="flex flex-col bg-white text-center rounded-3xl p-8 items-center absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2"
          v-show="isFinish"
        >
          <p class="m-3 break-all">
            You've finished all quizes. Here's your score
          </p>

          <!-- circle-score -->
          <div class="bg-gray-300 rounded-full w-36 h-36 pt-9 mt-10">
            <p class="text-4xl font-extrabold">{{ score }}</p>
            <p class="font-medium">Out of {{ questions.length }}</p>
          </div>

          <!-- button-action -->
          <div
            class="flex md:flex-row-reverse gap-2 font-bold mt-5 py-4 w-full text-center"
          >
            <!--button-play-again -->
            <button
              class="bg-blue-500 text-white rounded-lg w-full py-2 hover:bg-blue-600 duration-200"
              @click="
                score = 0;
                currentQuestionNumber = 0;
                isFinish = false;
                getQuestions();
              "
            >
              Play again
            </button>
            <!--button-home -->
            <button
              @click="
                () => {
                  isPlaying = !isPlaying;
                  isFinish = false;
                  currentQuestionNumber = 0;
                  playAndPause();
                  selectedCategory = '';
                  selectedDifficulty = '';
                }
              "
              class="bg-gray-100 border border-black rounded-lg w-full p-2 py-2 hover:bg-gray-200 duration-200"
            >
              Home
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
