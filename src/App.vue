<script setup>
import { ref, computed } from "vue"
import moon from './components/icons/moon.vue'
import sun from './components/icons/sun.vue'

const showQuestion = ref(false);
const showResultPopup = ref(false)
const showHowtToPlay = ref(false)
const showAnswer = ref(false)

const question = ref([]);
const catagories = ref()
const score = ref(0)
const currentQuestion = ref(1)
const process = ref(10)

const warnMode = ref(false)
const nightMode = ref(false)

const category = ref('');
const difficulty = ref('');

const selectCTG = ref('');

const difficulties = ref([
  { name: 'Easy', description: 'easy', bg: 'bg-green-300', selectBg: 'bg-green-400' },
  { name: 'Medium', description: 'medium', bg: 'bg-amber-200', selectBg: 'bg-amber-400' },
  { name: 'Hard', description: 'hard sud sud', bg: 'bg-red-300', selectBg: 'bg-red-500' },
])

const choices = computed(() => {
  let choice = []
  if (question.value[currentQuestion.value - 1]) {
    choice = [
      ...question.value[currentQuestion.value - 1].incorrectAnswers, question.value[currentQuestion.value - 1].correctAnswer
    ]
    shuffle(choice)
  }
  return choice
})

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

const choiceOnClick = (answer, correctAnswer) => {
  showAnswer.value = true
  if (answer === correctAnswer) {
    score.value += 1
  }
  setTimeout(() => {
    currentQuestion.value += 1
    process.value += 10
    showAnswer.value = false
  }, 500)
}

// show result when user answer all question
const showResult = () => {
  if (currentQuestion.value === 10) {
    setTimeout(() => {
      showQuestion.value = false
      showResultPopup.value = true
    }, 500)

  }
}

// check that user select category and difficulty
const checkOption = () => {
  if (category.value === '' || difficulty.value === '') {
    warnMode.value = true
    showQuestion.value = false
  } else {
    warnMode.value = false
    showQuestion.value = true
    getQuestion(category.value, difficulty.value)
  }
}

function randomQuestion(questions) {
  const random = Math.floor(Math.random() * 10)
  return questions[random]
}

function shuffle(array) {
  return array?.sort(() => Math.random() - 0.5)
}

const darkModeSet = (item) => {
  if (nightMode.value === true) {
    if (item === 'block') {
      return 'bg-[#242731] text-white hover:bg-slate-700'
    }else if (item === 'font') {
      return 'text-white'
    }else if (item === 'border') {
      return 'border-white text-white'
    }else if (item === 'bg') {
      return 'bg-[#1F2128]'
    }else if (item === 'btn') {
      return 'bg-teal-500 text-white hover:bg-slate-600 hover:text-white'
    }
  }
}
</script>

<template>
  <div :class="nightMode ? darkModeSet('bg') : ''" class="h-full">

    <!-- night mode btn -->
    <div class="  fixed top-12 right-1 md:right-8">
      <label class="swap swap-rotate neutral justify-center mr-6  w-10 h-10 rounded-full shadow-lg"
        :class="nightMode ? darkModeSet('block') : 'bg-slate-200'">
        <input type="checkbox" v-model="nightMode" />
        <moon class="swap-on w-5 h-5 " />
        <sun class="swap-off w-5 h-5" />
      </label>
    </div>

    <!-- header -->
    <div class="w-full h-screen flex items-center justify-center" id="head">
      <div class="md:grid md:grid-cols-2 content-center w-full">
        <div class="flex flex-col justify-center md:ml-24 text-center md:text-left">
          <h1 :class="nightMode ? darkModeSet('font') : 'text-black'"
            class="text-3xl  md:text-left flex justify-center  md:mr-6 font-bold font-mono  md:text-6xl">
            Quiz ja pak quiz tueng quiz kat
          </h1>
          <p class=" md:text-left mt-8 " :class="nightMode ? darkModeSet('font') : 'text-black'">A quiz IQ with this
            quiz
          </p>

          <div class="flex flex-row mt-5 md:justify-start  justify-center">
            <a href="#catagory">
              <button :class="nightMode ? darkModeSet('font') : 'text-black'"
                class="bg-teal-500 p-2 pl-4 pr-4 mt-4  rounded-lg hover:bg-black hover:text-white shadow-lg">
                get start
              </button>
            </a>
            <button :class="nightMode ? darkModeSet('border') : 'border-black text-black'"
              class=" bg-transparent border  p-2 pl-4 pr-4 mt-4  rounded-lg hover:bg-black hover:text-white ml-3 shadow-lg"
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
      <div class="bg-slate-200/90  rounded-lg text-black w-3/4 relative shadow-2xl h-5/6">
        <button class="absolute right-0 p-6 hover:underline" @click="showHowtToPlay = !showHowtToPlay">close</button>
        <div class="text-center pt-28 space-y-9">
          <h1 class="text-3xl md:text-5xl font-mono">How to play Inw Trivia</h1>
          <p>coming soon</p>
        </div>
      </div>
    </div>



    <!-- catagories -->
    <div class="flex justify-center w-full" id="catagory">
      <div>
        <h1 class="text-center text-3xl  font-bold mt-14" :class="nightMode ? darkModeSet('font') : 'text-black'">
          Choose a catagory</h1>
        <div class=" text-center mt-10 grid md:grid-cols-3 gap-4  grid-cols-2">
          <a href="#level" v-for="(cg, key) in catagories"
            @click="category = catagories[key].reduce((a, b) => a.length > b.length ? a : b); selectCTG=key"
            :class="catagories[key].includes(category) ? 'bg-slate-700 text-white' : 'bg-slate-300' && nightMode ? darkModeSet('block') : 'bg-slate-300 text-black hover:bg-black hover:text-white'"
            class="px-2   w-32 h-28 md:w-52 text-sm pt-12 rounded-lg  shadow-xl">
            <div class="flex justify-center">{{ key }}</div>
          </a>
        </div>
      </div>
    </div>



    <!-- level -->
    <div class="flex justify-center w-full cursor-pointer" id="level">
      <div>
        <h1 class="text-center text-3xl  font-bold mt-14" :class="nightMode ? darkModeSet('font') : 'text-black'">
          Which level do you want</h1>
        <div class="mt-10 md:grid md:grid-cols-3 gap-4 flex flex-col items-center">
          <a href="#letgo" v-for="level in difficulties" @click="difficulty = level.name.toLowerCase()"
            :class="level.name.toLowerCase() === difficulty ? level.selectBg : level.bg"
            class="px-2 text-black   text-sm pt-12  h-64 w-52 rounded-lg hover:bg-black hover:text-white relative shadow-lg">
            <div class="flex justify-center ">
              <div class="absolute bottom-4 left-4">
                <p>{{ level.name }}</p>
                <p>{{ level.description }}</p>
              </div>
            </div>
          </a>
        </div>
        <!-- let's play btn -->
        <a href="#question">
          <div class="flex justify-end" id="letgo">
            <!-- <p class="text-black flex justify-center">Category: {{ category }} , Level: {{ difficulty }}</p> -->
            <button class=" rounded-lg p-1.5 mt-4 w-28 mb-14 shadow-lg"
              :class="nightMode ? darkModeSet('btn') : 'bg-black text-white'" @click="checkOption()">
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
    <div v-show="showQuestion" class="h-screen mt-20">

      <!-- nav bar -->
      <div class="md:flex justify-center hidden">
        <div id="navbar" class=" rounded-2xl p-4 w-5/6 grid grid-cols-3 shadow-lg mt-9"
          :class="nightMode ? darkModeSet('block') : 'bg-slate-200'">
          <div class="cursor-pointer flex justify-start hover:bg-slate-500/20 w-fit h-7 rounded-lg items-center">
            <a href="#head">
              <p class=" p-2" :class="nightMode?darkModeSet('font'):'text-black'">back</p>
            </a>
          </div>
          <div class="flex justify-center cursor-pointer">
            <p class="text-black text-center bg-green-300 rounded-lg px-2 grid content-center">score {{
              score
            }}</p>
          </div>
          <div class="flex justify-end mr-2 cursor-pointer ">
            <p :class="nightMode?darkModeSet('font'):'text-black'">Inw Trivia</p>
          </div>

        </div>
      </div>


      <!-- question -->
      <div class=" flex flex-col items-center md:mt-20 mt-10 justify-center mb-10 " id="question">
        <div class="text-center text-black space-y-6 w-3/5 break-words mt-10">
          <div class=" p-6 rounded-2xl tex-center grid content-center shadow-lg"
            :class="nightMode ? darkModeSet('block') : 'bg-slate-200'">
            <p class="text-sm">Question {{ currentQuestion }}/10 &nbsp; <span class=" md:hidden">
                score {{ score }}</span></p>
            <h1 class="text-2xl  md:text-3xl break-words">{{ question[currentQuestion - 1]?.question }}</h1>
          </div>
          <div class="grid grid-cols-1 md:grid-cols-2  rounded-2xl p-2 shadow-lg"
            :class="nightMode ? darkModeSet('block') : 'bg-slate-200'">
            <div v-for="(choice, index) in choices" :key="index" :id="choice"
              @click="choiceOnClick(choice, question[currentQuestion - 1]?.correctAnswer); showResult()"
              :class="showAnswer ? question[currentQuestion - 1]?.correctAnswer === choice ? 'bg-green-400' : 'bg-red-500' : 'bg-teal-500'"
              class=" p-2 pl-4 pr-4 rounded-lg m-2 hover:bg-black hover:text-white w-84 shadow-lg">
              <button>
                {{ choice }}
              </button>
            </div>
          </div>
          correct: {{ question[currentQuestion - 1]?.correctAnswer }}
          <div>
            <button class=" rounded-full w-10 h-10 shadow-xl"
              :class="nightMode ? darkModeSet('block') : 'bg-slate-200'"
              @click="currentQuestion = 1; score = 0; process = 10">RL</button>
          </div>
        </div>
      </div>
      <!-- step -->
      <!-- <div class="flex justify-center">
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
        <div class="flex justify-center w-3/5">
          <progress class="progress progress-accent" :class="nightMode ? 'bg-white' : ''" :value="process"
            max="100"></progress>
        </div>
      </div>

      <!-- ref -->
      <div class="flex justify-center mt-4 ">
        <p :class="nightMode?darkModeSet('font'): 'text-black'">All question are from <a href="https://the-trivia-api.com/">"The Trivia API"</a></p>
      </div>
    </div>

    <!-- result -->
    <div class=" h-screen bg-black/50 fixed  top-0 left-0 right-0 z-50 w-full" v-show="showResultPopup">
      <div class=" fixed top-1/2 left-1/2 -translate-y-1/2 -translate-x-1/2">
        <div class="relative w-full h-full max-w-2xl md:h-auto grid content-center">
          <!-- content -->
          <div class="relative bg-white rounded-3xl shadow text-black grid content-center p-6">
            <div class="flex flex-col items-start justify-between p-4 space-y-2 rounded-t dark:border-gray-600">
              <h3 class="text-2xl  text-black font-bold">
                You've finished all quizes. Here's your score.
              </h3>
              <p class="text-sm">Category : {{ selectCTG }} </p>
              <p class="text-sm">Level : <span :class="'text-teal-600'">{{ difficulty.toUpperCase() }}</span></p>
            </div>
            <div class="flex justify-center">
              <div class=" leading-none p-6 space-y-6 bg-slate-200 w-36 h-36 rounded-full text-center">
                <p class="font-bold text-5xl">{{ score }}</p>
                <p class="leading-none">Out of 10</p>
              </div>
            </div>
            <div class="flex items-center p-6 space-x-2  justify-center rounded-b dark:border-gray-600">
              <a href="#head">
                <button
                  @click="showResultPopup = !showResultPopup ; score = 0; category = ''; difficulty = '', currentQuestion = 1; process = 10"
                  class="text-black bg-white border border-black hover:bg-black  hover:text-white font-medium rounded-lg text-sm px-5 py-2.5 text-center">
                  Home</button>
              </a>

              <a href="#catagory">
                <button
                  @click="showResultPopup = !showResultPopup ; score = 0; category = ''; difficulty = '', currentQuestion = 1; process = 10"
                  class="text-black bg-teal-500 hover:bg-black  font-medium rounded-lg text-sm px-5 py-2.5 text-center hover:text-white">
                  Play again</button></a>
            </div>
          </div>
        </div>
      </div>
    </div>


  </div>
</template>

<style scoped>

</style>