<script setup>
import { ref } from 'vue'
import RiArrowLeftSLine from './components/icons/RiArrowLeftSLine.vue'

fetch('https://the-trivia-api.com/api/categories')
.then(response => response.json())
.then(data => categories.value=data)

const questions = ref([])
const categories = ref([])
const quizCompleted = ref(false)
const currentQuestion = ref(0)
const showHowtToPlay = ref(false)
const category =ref('')
const difficulty = ref('')
const selectedLevel = ref(difficulty)


const getQuestions = (category, difficulty) => {
  fetch(`https://the-trivia-api.com/api/questions?categories=${category}&limit=10&region=TH&difficulty=${difficulty}`)
  .then(response => response.json())
  .then(data => questions.value = data)
  return questions.value
}

// const nextQuestion = () => {
//   if(currentQuestion.value < questions.value.length -1) {
//     currentQuestion.value++
//   }
//   else{
//     quizCompleted.value = true
//   }
// }


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
    <div id="home" class="w-full h-screen flex items-center justify-center">
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
    <div id="categories" class="md:pt-3 py-7 px-10">
      <div class="md:text-4xl text-2xl md:none font-extrabold md:my-12 my-6">
        What <span class="text-red-700">kind</span> of quiz would you like to take ?
      </div>
      <div class="grid md:grid-cols-4 grid-cols-2 gap-5 md:px-48 font-medium">
        <!-- for...in ; got property of object -->
        <a href="#level" v-for="(prop,object) in categories"
        class="py-14 border rounded-lg bg-stone-200 hover:bg-green-200 shadow-lg" 
        @click="category = prop[0]">
        {{ object }}
        </a>
      </div>
    </div>

    <!-- LEVEL -->
    <div id="level" class="md:py-6 py-10 px-9">
      <div class="md:text-4xl text-2xl md:none font-extrabold md:my-14 my-7">What <span class="text-red-700">difficulty</span> would you like to start with ?</div>
      <div class="grid md:grid-cols-3 grid-cols-1 gap-7 md:px-32 px-5 font-medium">
        <div id="easy" class="h-96 border rounded-3xl bg-green-400 hover:bg-green-600 shadow-xl px-9 pt-64 text-start" 
        @click="difficulty = 'easy'" :class="selectedLevel == 'easy' ? 'text-white  font-extrabold' : 'text-black'">
          <p class="text-3xl font-extrabold">Easy</p>
          <p class="">A piece of cake.</p>          
        </div>
        <div id="medium" class="h-96 border rounded-3xl bg-yellow-300 hover:bg-yellow-600 shadow-xl px-9 pt-64 text-start" 
        @click="difficulty = 'medium'" :class="selectedLevel == 'medium' ? 'text-white font-extrabold' : 'text-black'">
          <p class="text-3xl font-extrabold">Medium</p>
          <p class="">Time to put your skill to the test!</p>
        </div>
        <div id="hard" class="h-96 border rounded-3xl bg-red-500 hover:bg-red-700 shadow-xl px-9 pt-64 text-start" 
        @click="difficulty = 'hard'" :class="selectedLevel == 'hard' ? 'text-white  font-extrabold' : 'text-black'">
          <p class="text-3xl font-extrabold">Hard</p>
          <p class="">Your're smart? Prove it with this one!</p>
        </div>
      </div>
      <div class="flex md:justify-between justify-end mt-10 md:px-32 md:none m-5">        
        <a href="#categories" class="md:block hidden"><button class="border border-black rounded-lg py-1 px-5 font-semibold tracking-wide shadow-lg">Back</button></a>
        <a href="#test"><button class="border rounded-lg py-1 px-5 bg-blue-500 text-white font-bold tracking-wide shadow-lg">Lets Play</button></a>
      </div>
      <p>Selected : {{ category }}</p>
      <p>level : {{ difficulty }}</p>
    </div>

    <!-- BAR -->
    <div class="h-screen">
    <div id="test" class="md:py-10 py-5  md:px-20 px-4">
          <div class="flex flex-row border rounded-full bg-stone-100 p-1 shadow-lg">
            <a href="#home"><button class="flex flex-row items-center border rounded-full p-2 bg-white "><RiArrowLeftSLine/>Back</button></a>
          </div>
        </div>

    <!-- question -->
    <div class="flex justify-center">
          <div class="md:w-2/4 m-9 bg-white border rounded-xl shadow-xl">
            <p class="font-serif mt-6 text-stone-400">Question {{  }} / {{ questions.length }} </p>
            <h1 class="text-xl font-semibold m-10 " >{{ getCurrentQuestion }}</h1>
          </div>
        </div> 

          <!-- choices -->    
        <!-- <div class="grid md:grid-cols-2 grid-cols-1 justify-center mx-10 gap-5">
          <div v-for="(choice) in getCurrentQuestion.incorrectAnswers">
            <div class="bg-white border rounded-xl shadow-xl py-3 hover:bg-slate-200" @click="nextQuestion">
              <p>{{ choice }}</p>            
            </div>
          </div>
          
        </div> -->
        </div>

  </div>

</template>
 
<style scoped>


</style>