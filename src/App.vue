<script setup>
import { ref } from "vue"
import Moon from "@/components/icon/MaterialSymbolsDarkModeOutline.vue";
import Sun from "@/components/icon/IconParkOutlineDarkMode.vue";
import colors from "tailwindcss/colors";
const isLevelSelect = ref(false)
const showQuestion = ref(false);
const category = ref('');
const difficulty = ref('');
const question = ref([]);
const questionNum = ref(0)
const q = ref('')
const ques = ref([])
const ch = ref([])
const isPopup = ref(false)
const userAnswer = ref('')
const process = ref(10)
const catagories = ref()
const showHowtToPlay = ref(false)
const quizComplete = ref(false)
const closeQuiz = ref(true)
const score = ref(0)
const nightMode = ref(false)
const currentIndex = ref(0)
const correctOrNot = ref('')
const warnMode = ref(false)

fetch('https://the-trivia-api.com/api/categories')
    .then((response) => response.json())
    .then((data) => (catagories.value = data))
function getQuestion(category, difficulty) {
  fetch(
      `https://the-trivia-api.com/api/questions?categories=${category}&limit=10&region=TH&difficulty=${difficulty}`
  )
      .then((response) => response.json())
      .then((data) => (question.value = data))
      .catch((error) => console.log(error))
  return randomQuestion(question.value)
}
function randomQuestion(questions) {
  const random = Math.floor(Math.random() * 10)
  return questions[random]
}
function shuffle(array) {
  return array?.sort(() => Math.random() - 0.5)
}
function getChoice(x) {
  const correct = x?.correctAnswer
  const incorrect = x?.incorrectAnswers
  const choice = []
  choice?.push(correct)
  incorrect?.forEach(element => {
    choice?.push(element)
  })
  return choice
}
const themeDark = ref({ 'background-color': '#3f3f46', 'color': '#FFFFFF' , 'border-color': '#666666'})
const themeWhite = ref({ 'background-color': '#f2f2f2', 'color': '#3f3f46' })
const blockDark = ref({ 'background-color': '#666666', 'color': '#f2f2f2' })
const fontDark = ref({ 'color': '#FFFFFF' })
</script>
<template>
  <div :style="nightMode ? themeDark : themeWhite">
    <!-- header -->
    <div class="w-full flex flex-row space-x-2 justify-end p-2">
      <label class=" flex flex-row rounded-full justify-end">
        <input type="checkbox" class="toggle" checked v-model="nightMode" />
        <moon class="swap-on  w-5 h-5" v-if="nightMode === true" />
        <sun class="swap-on  w-5 h-5" v-if="nightMode === false" />
      </label>
    </div>
    <div class="w-full h-screen flex items-center justify-center" id="head">
      <div class="md:grid md:grid-cols-2 content-center w-full">
        <div class="flex flex-col justify-center md:ml-24 text-center md:text-left">
          <h1  :style="nightMode ? fontDark : ''"
               class="text-3xl text-black md:text-left flex justify-center  md:mr-6 font-bold font-mono  md:text-6xl">
            Quiz ja pak quiz tueng quiz kat
          </h1>
          <p class=" md:text-left mt-8 text-black" :style="nightMode === true ? themeDark : ''">A quiz IQ with this quiz.</p>
          <div class="flex flex-row mt-5 md:justify-start  justify-center">
            <a href="#catagory">
              <button :style="nightMode ? fontDark : ''"
                      class="bg-teal-500 p-2 pl-4 pr-4 mt-4 text-black rounded-lg hover:bg-black hover:text-white shadow-lg">
                get start
              </button>
            </a>
            <button :style="nightMode ? themeDark : ''"
                    class=" bg-transparent border border-black p-2 pl-4 pr-4 mt-4 text-black rounded-lg hover:bg-black hover:text-white ml-3 shadow-lg"
                    @click="showHowtToPlay = !showHowtToPlay;">
              how to play
            </button>
          </div>
        </div>
        <div class="w-11/12 hidden md:block">
          <img src="./assets/image/cover.png" alt="quiz" class="">
        </div>
      </div>
    </div>
    <!-- How to play pop up-->
    <div class="fixed top-0 left-0 right-0 z-50 w-full bg-black/50 flex items-center justify-center h-screen"
         v-show="showHowtToPlay">
      <div class="bg-slate-200/90  rounded-lg text-black w-3/4 relative shadow-2xl h-5/6"  :style="nightMode ? blockDark : ''">
        <button class="absolute right-0 p-6 hover:underline" @click="showHowtToPlay = !showHowtToPlay">close</button>
        <div class="text-center pt-28 space-y-9" :style="nightMode ? fontDark : ''">
          <h1 class="text-3xl md:text-5xl font-mono">How to play Inw Trivia</h1>
          <p>coming soon</p>
        </div>
      </div>
    </div>
    <!-- catagories -->
    <div class="flex justify-center w-full" id="catagory">
      <div>
        <h1 class="text-center text-3xl text-black font-bold mt-14" :style="nightMode ? fontDark : ''">Choose a catagory</h1>
        <div class=" text-center mt-10 grid md:grid-cols-3 gap-4  grid-cols-2">
          <a href="#level" v-for="(cg, key, index) in catagories" @click="category = cg[0]"
             :style="nightMode ? blockDark : ''"
             class="px-2 text-black bg-slate-300 w-32 h-28 md:w-52 text-sm pt-12 rounded-lg hover:bg-black hover:text-white shadow-xl">
            <div class="flex justify-center">{{ key }}</div>
          </a>
        </div>
      </div>
    </div>
    <!-- Level -->
    <div class="flex justify-center cursor-pointer" id="level">
      <div>
        <h1 class="text-center text-3xl text-black font-bold mt-10" :style="nightMode ? fontDark : ''">Which level do you want</h1>
        <div class="mt-10 md:grid md:grid-cols-3 gap-4 flex flex-col items-center">
          <a href="#letgo">
            <div :class="isLevelSelect ? 'bg-black text-white' : 'text-black bg-green-300'"
                 class="h-64 w-52 rounded-lg hover:bg-black hover:text-white relative shadow-lg" @click="difficulty = 'easy';
              // isLevelSelect = !isLevelSelect;
              ques = getQuestion(category, difficulty); q = ques?.question; ch = getChoice(ques);">
              <div class="absolute bottom-4 left-4"  :style="nightMode ? fontDark : ''">
                <p>Easy</p>
                <p>Description</p>
              </div>
            </div>
          </a>
          <a href="#letgo">
            <div :class="isLevelSelect ? 'bg-black text-white' : 'text-black bg-amber-200'"
                 class="h-64 w-52 rounded-lg hover:bg-black hover:text-white relative shadow-lg" @click="difficulty = 'medium';
              // isLevelSelect = !isLevelSelect;
              ques = getQuestion(category, difficulty); q = ques?.question; ch = getChoice(ques);">
              <div class="absolute bottom-4 left-4" :style="nightMode ? fontDark : ''">
                <p>Medium</p>
                <p>Description</p>
              </div>
            </div>
          </a>
          <a href="#letgo">
            <div :class="isLevelSelect ? 'bg-black text-white' : 'text-black bg-red-400'"
                 class="h-64 w-52 rounded-lg hover:bg-black hover:text-white relative shadow-lg" @click="difficulty = 'hard';
              // isLevelSelect = !isLevelSelect;
              ques = getQuestion(category, difficulty); q = ques?.question; ch = getChoice(ques);">
              <div class="absolute bottom-4 left-4" :style="nightMode ? fontDark : ''">
                <p>Hard</p>
                <p>Description</p>
              </div>
            </div>
          </a>
        </div>
        <a href="#question">
          <div class="flex justify-end" id="letgo">
            <!-- <p class="text-black flex justify-center">Category: {{ category }} , Level: {{ difficulty }}</p> -->
            <button :style="nightMode ? fontDark : ''"
                    class="bg-teal-500 p-2 pl-4 pr-4 mt-4 text-black rounded-lg hover:bg-black hover:text-white shadow-lg"
                    @click="category === '' || difficulty === '' ? warnMode = !warnMode : ''; category === '' || difficulty === '' ? showQuestion = !showQuestion : ''; difficulty === null ? warnMode = !warnMode : ques = getQuestion(category, difficulty); q = ques?.question; ch = getChoice(ques); questionNum = 1; showQuestion = !showQuestion;">
              Let's play
            </button>
          </div>
        </a>
      </div>
    </div>
    <!-- warn -->
    <div class="fixed top-0 left-0 right-0 z-50  p-4 grid  place-content-center bg-black/50 h-screen" v-show="warnMode">
      <div class="relative w-full h-full max-w-md md:h-auto ">
        <div class="relative bg-white rounded-lg shadow dark:bg-gray-700 ">
          <button type="button" @click="warnMode = !warnMode"
                  class="absolute top-3 right-2.5 text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center dark:hover:bg-gray-800 dark:hover:text-white"
                  data-modal-hide="popup-modal">
            <svg aria-hidden="true" class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20"
                 xmlns="http://www.w3.org/2000/svg">
              <path fill-rule="evenodd"
                    d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                    clip-rule="evenodd"></path>
            </svg>
            <span class="sr-only">Close modal</span>
          </button>
          <div class="p-6 text-center">
            <svg aria-hidden="true" class="mx-auto mb-4 text-gray-400 w-14 h-14 dark:text-gray-200" fill="none"
                 stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path>
            </svg>
            <h3 class="mb-5 text-lg font-normal text-gray-500 dark:text-gray-400">Please select category or level</h3>
          </div>
        </div>
      </div>
    </div>
    <!-- question block -->
    <div v-show="showQuestion" class="h-screen">
      <!-- nav bar -->
      <div class="md:flex justify-center hidden">
        <div id="navbar" class="bg-slate-200 rounded-2xl p-4 w-5/6 grid grid-cols-3 shadow-lg mt-9" v-show="closeQuiz" :style="nightMode ? blockDark : ''">
          <p class="text-black flex justify-start" :style="nightMode ? fontDark : ''">back</p>
          <div class="flex justify-center">
            <p class="text-black text-center bg-green-300 rounded-lg px-2 grid content-center" :style="nightMode ? fontDark : ''">score {{
                score
              }}</p>
          </div>
          <label class=" flex flex-row rounded-full justify-end">
            <input type="checkbox" class="toggle" checked v-model="nightMode" />
            <moon class="swap-on  w-5 h-5" v-if="nightMode === true" />
            <sun class="swap-on  w-5 h-5" v-if="nightMode === false" />
          </label>
        </div>
      </div>
      <!-- question -->
      <div class=" flex flex-col items-center md:mt-40 mt-10 justify-center  mb-20 " v-show="closeQuiz" id="question">
        <div class="text-center text-black space-y-6 w-3/5 break-words">
          <div class="bg-slate-200 p-6 rounded-2xl tex-center grid content-center shadow-lg" :style="nightMode ? blockDark : ''">
            <p class="text-sm">Question {{ questionNum }}/10 &nbsp; <span class=" md:hidden">
                score {{ score }}</span></p>
            <h1 class="text-2xl  md:text-3xl break-words">{{ q }}</h1>
          </div>
          <div class="grid grid-cols-1 md:grid-cols-2 bg-slate-200 rounded-2xl p-2 shadow-lg" :style="nightMode ? { 'background-color': '#404040' } : ''">
            <div v-for="(c, index) in shuffle(ch)" :key="index" :id="c"
                 @click="userAnswer = c; userAnswer === ques?.correctAnswer ? score++ : ''; ++questionNum; process += 10; questionNum > 10 ? quizComplete = !quizComplete : '';
              ques = getQuestion(category, difficulty); q = ques?.question; ch = getChoice(ques); questionNum > 10 ? closeQuiz = !closeQuiz : ''
              userAnswer === ques?.correctAnswer ? correctOrNot = true : correctOrNot = false; currentIndex = questionNum; questionNum > 10 ? isPopup = !isPopup : ''"
                 class="bg-slate-500 p-2 pl-4 pr-4 rounded-lg m-2 hover:bg-black hover:text-white w-84 shadow-lg"  :style="nightMode ? blockDark : ''">
              <button>
                {{ c }}
              </button>
            </div>
          </div>
        </div>
      </div>
      <!-- step -->
      <!-- <div class="flex justify-center" v-show="closeQuiz">
          <ul class="steps">
            <li class="step" :class="questionNum > 1 ? 'step-accent' : ''"></li>
            <li class="step" :class="questionNum > 2 ? 'step-accent' : ''"></li>
            <li class="step" :class="questionNum > 3 ? 'step-accent' : ''"></li>
            <li class="step" :class="questionNum > 4 ? 'step-accent' : ''"></li>
            <li class="step" :class="questionNum > 5 ? 'step-accent' : ''"></li>
            <li class="step" :class="questionNum > 6 ? 'step-accent' : ''"></li>
            <li class="step" :class="questionNum > 7 ? 'step-accent' : ''"></li>
            <li class="step" :class="questionNum > 8 ? 'step-accent' : ''"></li>
            <li class="step" :class="questionNum > 9 ? 'step-accent' : ''"></li>
            <li class="step" :class="questionNum > 10 ? 'step-accent' : ''"></li>
          </ul>
        </div> -->
      <!-- progress bar-->
      <div class="flex justify-center">
        <div class="flex justify-center w-3/5" v-show="closeQuiz">
          <progress class="progress progress-accent" :value="process" max="100"></progress>
        </div>
      </div>
      <!-- ref -->
      <div v-show="closeQuiz" class="flex justify-center mt-4 mb-9">
        <p class="text-black" :style="nightMode ? fontDark : ''">All question are from <a href="https://the-trivia-api.com/">"The Trivia API"</a></p>
      </div>
      <!-- end quiz -->
      <!-- <div class="w-full h-screen">
        <div class="flex flex-col " v-show="quizComplete">
          <div class="flex justify-center">
            <h1 class="text-5xl text-black mt-28">Tum Sed Laew Ja See The Score</h1>
          </div>
          <div class="flex justify-center mt-14">
            <button class="bg-black text-white rounded-lg p-1.5 w-32"
              @click="isPopup = !isPopup; quizComplete = !quizComplete; showQuestion = !showQuestion; closeQuiz = !closeQuiz">
              see the result
            </button>
          </div>
        </div>
      </div> -->
    </div>
    <!-- result -->
    <div class=" h-screen bg-black/50 fixed  top-0 left-0 right-0 z-50 w-full" v-show="isPopup">
      <div class=" fixed top-1/2 left-1/2 -translate-y-1/2 -translate-x-1/2">
        <div class="relative w-full h-full max-w-2xl md:h-auto grid content-center">
          <!-- content -->
          <div class="relative bg-white rounded-3xl shadow text-black grid content-center p-6"  :style="nightMode ? themeDark : ''">
            <div class="flex items-start justify-between p-4  rounded-t dark:border-gray-600">
              <h3 class="text-xl  text-black " :style="nightMode ? fontDark : ''">
                You've finished all quizes. Here's your score.
              </h3>
            </div>
            <div class="flex justify-center">
              <div class=" leading-none p-6 space-y-6 bg-slate-200 w-36 h-36 rounded-full text-center" :style="nightMode ? blockDark : ''">
                <p class="font-bold text-5xl">{{ score }}</p>
                <p class="leading-none">Out of 10</p>
              </div>
            </div>
            <div class="flex items-center p-6 space-x-2  justify-center rounded-b dark:border-gray-600">
              <button @click="isPopup = !isPopup"
                      :style="nightMode ? themeDark : ''"
                      class="bg-transparent text-black bg-white border border-black hover:bg-black hover:text-white font-medium rounded-lg text-sm px-5 py-2.5 text-center shadow-lg">
                Close</button>
              <a href="#head">
                <button @click="isPopup = !isPopup ; score = 0"
                        class="text-black bg-teal-500 hover:bg-black  font-medium rounded-lg text-sm px-5 py-2.5 text-center hover:text-white">
                  Play again</button></a>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- result -->
    <!-- <div class="flex justify-center mb-10 fixed top-0 left-0 right-0   h-screen w-full p-4  md:h-full bg-black/50"
      v-show="isPopup">
      <div
        class=" bg-slate-200/90 top-52 rounded-lg text-black w-3/4 h-2/5 relative shadow-2xl grid place-content-center">
        <button class="absolute right-0 p-6 hover:underline" @click="isPopup = !isPopup">close</button>
        <h1 class="grid h-screen place-items-center text-xl md:text-4xl ">Your Score is {{ score }}/{{ 10 }}
        </h1>
        <a href="#head">
          <button class="absolute bottom-24  md:bottom-8 left-8 right-8 md:left-24 md:right-24  bg-black text-white
           rounded-full p-2 md:p-4 text-sm md:text-2xl hover:underline " @click="isPopup = !isPopup; score = 0">
            Play again</button>
        </a>
      </div>
    </div> -->
  </div>
</template>
<style scoped>
.toggle {
  display: none;
}
</style>
