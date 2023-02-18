<script setup>
import { ref, computed } from "vue";
import moon from "./components/icons/moon.vue";
import sun from "./components/icons/sun.vue";

const showQuestion = ref(false);
const showResultPopup = ref(false);
const showHowtToPlay = ref(false);
const showAnswer = ref(false);

const question = ref([]);
const catagories = ref();
const score = ref(0);
const currentQuestion = ref(1);
const process = ref(10);

const warnMode = ref(false);
const nightMode = ref(false);

const category = ref("");
const difficulty = ref("");

const selectCTG = ref("");

const difficulties = ref([
  {
    name: "Easy",
    description: "easy",
    bg: "bg-green-300",
    selectBg: "bg-green-400",
  },
  {
    name: "Medium",
    description: "medium",
    bg: "bg-amber-200",
    selectBg: "bg-amber-400",
  },
  {
    name: "Hard",
    description: "hard sud sud",
    bg: "bg-red-300",
    selectBg: "bg-red-500",
  },
]);

const choices = computed(() => {
  let choice = [];
  if (question.value[currentQuestion.value - 1]) {
    choice = [
      ...question.value[currentQuestion.value - 1].incorrectAnswers,
      question.value[currentQuestion.value - 1].correctAnswer,
    ];
    shuffle(choice);
  }
  return choice;
});

fetch("https://the-trivia-api.com/api/categories")
  .then((response) => response.json())
  .then((data) => (catagories.value = data));

function getQuestion(category, difficulty) {
  fetch(
    `https://the-trivia-api.com/api/questions?categories=${category}&limit=10&region=TH&difficulty=${difficulty}`
  )
    .then((response) => response.json())
    .then((data) => (question.value = data))
    .catch((error) => console.log(error));

  return randomQuestion(question.value);
}

//set for start quiz again
const setNewQuiz = () => {
  showResultPopup.value = false;
  score.value = 0;
  category.value = "";
  difficulty.value = "";
  currentQuestion.value = 1;
  process.value = 10;
};

// check user answer and go to next question
const choiceOnClick = (answer, correctAnswer) => {
  showAnswer.value = true;
  if (answer === correctAnswer) {
    score.value += 1;
  }
  setTimeout(() => {
    currentQuestion.value += 1;
    process.value += 10;
    showAnswer.value = false;
  }, 500);
};

// show result when user answer all question
const showResult = () => {
  if (currentQuestion.value === 10) {
    setTimeout(() => {
      showQuestion.value = false;
      showResultPopup.value = true;
    }, 500);
  }
};

// check that user select category and difficulty
const checkOption = () => {
  if (category.value === "" || difficulty.value === "") {
    warnMode.value = true;
    showQuestion.value = false;
  } else {
    warnMode.value = false;
    showQuestion.value = true;
    getQuestion(category.value, difficulty.value);
  }
};

function randomQuestion(questions) {
  const random = Math.floor(Math.random() * 10);
  return questions[random];
}

function shuffle(array) {
  return array?.sort(() => Math.random() - 0.5);
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
            <a href="#catagory">
              <button
                :class="nightMode ? darkModeSet('font') : 'text-black'"
                class="bg-teal-500 p-2 pl-4 pr-4 mt-4 rounded-lg hover:bg-black hover:text-white shadow-lg"
              >
                get start
              </button>
            </a>
            <button
              :class="
                nightMode ? darkModeSet('border') : 'border-black text-black'
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
</template>

<style scoped></style>
