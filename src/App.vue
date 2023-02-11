<script setup>
import {ref , computed} from 'vue'
import RiArrowLeftSLine from './components/icons/RiArrowLeftSLine.vue'

fetch('https://the-trivia-api.com/api/questions?limit=20&categories=science,history')
  .then(response => response.json())
  .then(data => questions.value=data)

fetch('https://the-trivia-api.com/api/categories')
.then(response => response.json())
.then(data => categories.value=data)

const questions = ref([])
const categories = ref([])
const quizCompleted = ref(false)
const currentQuestion = ref(0)

const selected = null

const showHowtToPlay = ref(false)

// const choices = []





// Cal score
// const score = computed( () => {
//   let point = 0
//   let selected = null
//   questions.value.map( (q) => {
//     if( q.selected == q.correctAnswer){
//       point++
//     }
//   })
// })

const getCurrentQuestion = computed( () => {
  let question = questions.value[currentQuestion.value]
  question.index = currentQuestion.value
  return question 
})

const nextQuestion = () => {
  if(currentQuestion.value < questions.value.length -1) {
    currentQuestion.value++
  }
  else{
    quizCompleted.value = true
  }
}

const shuffle = (array) => {
  let currentIndex = array.length,
    temporaryValue,
    randomIndex;

  // While there remain elements to shuffle...
  while (0 !== currentIndex) {
    // Pick a remaining element...
    randomIndex = Math.floor(Math.random() * currentIndex);
    currentIndex -= 1;

    // And swap it with the current element.
    temporaryValue = array[currentIndex];
    array[currentIndex] = array[randomIndex];
    array[randomIndex] = temporaryValue;
  }

  return array;
};

  

</script>
 
<template>

  <!-- FULL PAGE -->
  <div class="text-center">

    <!-- HOME PAGE -->
    <!-- <div class="h-screen pt-56 bg-gray-300">
      <div class="text-4xl font-extrabold ">Quiz ja pak kid tueng Quiz Kat</div>
      <p class="text-xl italic font-semibold my-5">A quiz IQ with this Quiz</p>
      <div class="grid md:grid-cols-2 grid-cols-1 md:gap-5 gap-2">
        <a href="#categories" class="md:text-right"><button class="border rounded-lg px-10 py-1 border-blue-500 bg-blue-500 text-white font-bold tracking-wide shadow-lg">Get Start</button></a>
        <a href="#howto" class="md:text-left"><button class="border rounded-lg px-10 py-1 border-black font-semibold shadow-lg">How to play</button></a>
      </div>
    </div> -->

    <div class="w-full h-screen flex items-center justify-center">
      <div class="flex flex-col items-center ">
        <h1 class="text-4xl text-black text-center ml-2 mr-2 md:ml-6 md:mr-6 font-bold font-mono md:text-center md:text-6xl">
          Quiz ja pak quiz tueng quiz kat
        </h1>
        <p class="mt-8 text-black">A quiz IQ with this quiz</p>
        <div class="flex flex-row mt-5">
          <a href="#categories">
            <button
              class="bg-teal-500 p-2 pl-4 pr-4 mt-4 text-black rounded-lg hover:bg-black hover:text-white shadow-lg">
              Get start
            </button>
          </a>
          <button
            class="bg-white border border-black p-2 pl-4 pr-4 mt-4 text-black rounded-lg hover:bg-black hover:text-white ml-3 shadow-lg"
            @click="showHowtToPlay = !showHowtToPlay">
            How to play
          </button>
        </div>
      </div>
    </div>

    <!-- How to play -->
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


    <!-- CATEGORIES -->
    <div id="categories" class="py-7 px-10">
      <div class="md:text-4xl text-2xl md:none font-extrabold md:my-12 my-6">What <span class="text-red-700">kind</span> of quiz would you like to take ?</div>
      <div class="grid md:grid-cols-4 grid-cols-2 gap-5 md:px-48 font-medium">
        <a href="#level" v-for="(key) of Object.entries(categories)" 
        class="py-14 border rounded-lg bg-stone-200 hover:bg-green-200 shadow-lg">
        {{ key[0] }}
        </a>
      </div>
    </div>

    <!-- LEVEL -->
    <div id="level" class="py-6 px-9">
      <div class="md:text-4xl text-2xl md:none font-extrabold md:my-14 my-7">What <span class="text-red-700">difficulty</span> would you like to start with ?</div>
      <div class="grid md:grid-cols-3 grid-cols-1 gap-7 md:px-32 px-5 font-medium">
        <div class="h-96 border rounded-3xl bg-green-400 hover:bg-green-500 shadow-xl px-9 pt-64 text-start">
          <p class="text-3xl font-extrabold">Easy</p>
          <p class="">A piece of cake.</p>
        </div>
        <div class="h-96 border rounded-3xl bg-yellow-300 hover:bg-yellow-400 shadow-xl px-9 pt-64 text-start">
          <p class="text-3xl font-extrabold">Medium</p>
          <p class="">Time to put your skill to the test!</p>
        </div>
        <div class="h-96 border rounded-3xl bg-red-500 hover:bg-red-600 shadow-xl px-9 pt-64 text-start">
          <p class="text-3xl font-extrabold">Hard</p>
          <p class="">Your're smart? Prove it with this one!</p>
        </div>
      </div>
      <div class="text-right mt-10 md:px-32">
        <a href="#test"><button class="border rounded-lg py-1 px-5 bg-blue-500 text-white font-bold tracking-wide shadow-lg">Lets Play</button></a>
      </div>
    </div>

    <!-- question -->
    <!-- <div class="flex justify-center">
          <div class="md:w-2/4 w-3/4 bg-white border rounded-xl shadow-xl">
            <p class="font-serif mt-6 text-stone-400">Question {{ getCurrentQuestion.index+1 }} / {{ questions.length }} </p>
            <h1 class="text-xl font-semibold m-10 " >{{ getCurrentQuestion.question }}</h1>
          </div>
        </div>  -->

          <!-- choices -->    
        <!-- <div class="grid md:grid-cols-2 grid-cols-1 justify-center m-10 gap-5">
          <div v-for="(choice, index) in getCurrentQuestion.incorrectAnswers" :key="index">
            <div class="bg-white border rounded-xl shadow-xl py-3 hover:bg-slate-200" >
              <p>{{ choice }}</p>
            </div>
          </div>
        </div> -->

  </div>

</template>
 
<style scoped>


</style>