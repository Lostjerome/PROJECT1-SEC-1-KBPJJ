<script setup>
import { ref , computed} from 'vue'
import BiVolumeMuteFill from './components/icons/BiVolumeMuteFill.vue'
import BiVolumeUpFill from './components/icons/BiVolumeUpFill.vue'


// QUESTION
const categories = ref([])
const questions = ref([])
const currentQuestion = ref(1)
const quizCompleted = ref(false)
const showHowtToPlay = ref(false)
const category =ref('')
const levelData =  [  {title: 'easy' , description : 'A piece of cake.' , bg: 'bg-green-500', selectedbg: 'bg-green-500 border-green-500 shadow-black'}, 
                      {title: 'medium' , description : 'Time to put your skill to the test!' , bg: 'bg-yellow-400', selectedbg: 'bg-yellow-400 border-yellow-400 shadow-black'},
                      {title: 'hard' , description : `Your're smart? Prove it with this one!` , bg: 'bg-red-500', selectedbg: 'bg-red-500 border-red-500 shadow-black'},] 
const difficulty = ref('')
const letsPlay = ref(false)
const ans = ref('')
const score = ref(0)

fetch('https://the-trivia-api.com/api/categories')
.then(response => response.json())
.then(data => categories.value = data)

const getQuestions = (category, difficulty) => {
  fetch(`https://the-trivia-api.com/api/questions?categories=${category}&limit=10&region=TH&difficulty=${difficulty}`)
  .then(response => response.json())
  .then(data => questions.value = data)
}

const getChoices = computed( () => {
  let choices = []
  if(questions.value[currentQuestion.value-1]){
    choices = [...questions.value[currentQuestion.value-1].incorrectAnswers, questions.value[currentQuestion.value-1].correctAnswer]
  }
  return choices
})

// const checkAns = () => {
//   if(ans.value == questions.value[currentQuestion.value-1].correctAnswer){
//     score.value++
//   }
//   return score.value
// }

const nextQuestion = () => {
  if(currentQuestion.value < questions.value.length) {
    currentQuestion.value++
  }
  else{
    quizCompleted.value = true
  }
}

// bg-Music
const inputMusic = ref(null)
const defaultMusic = ('gameShow.mp3')
const isMusicPlay = ref(false)
const playAndPause = () => {
  isMusicPlay.value = !isMusicPlay.value
  if(isMusicPlay.value) inputMusic.value.play()
  else inputMusic.value.pause()
}

// const shuffle = (array) => {
//   let currentIndex = array.length,
//     temporaryValue,
//     randomIndex;

//   // While there remain elements to shuffle...
//   while (0 !== currentIndex) {
//     // Pick a remaining element...
//     randomIndex = Math.floor(Math.random() * currentIndex);
//     currentIndex -= 1;

//     // And swap it with the current element.
//     temporaryValue = array[currentIndex];
//     array[currentIndex] = array[randomIndex];
//     array[randomIndex] = temporaryValue;
//   }

//   return array;
// };

</script>
 
<template>

  <!-- FULL PAGE -->
  <div class="text-center" v-show="letsPlay = !letsPlay">

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
        class="py-14 border rounded-lg shadow-lg" 
        @click="category = prop[0]" :class="category === prop[0] ? 'border-stone-200 bg-stone-200 shadow-stone-500' : 'bg-stone-200'">
        {{ object }}
        </a>
      </div>
    </div>

    <!-- LEVEL -->
    <div id="level" class="md:py-6 py-10 px-9">
      <div class="md:text-4xl text-2xl md:none font-extrabold md:my-14 my-7">What <span class="text-red-700">difficulty</span> would you like to start with ?</div>

      <div class="grid md:grid-cols-3 grid-cols-1 gap-7 md:px-32 px-5 font-medium">
        <div id="easy" class="h-96 border rounded-3xl shadow-xl px-9 pt-64 text-start" 
        v-for="level in levelData"
        @click="difficulty = level.title" :class="difficulty === level.title ? level.selectedbg : level.bg">
          <p class="text-3xl font-extrabold">{{ level.title.toLocaleUpperCase() }}</p>
          <p class="">{{ level.description }}</p>          
        </div>
      </div>

      <div class="flex md:justify-between justify-end mt-10 md:px-32 md:none m-5">        
        <a href="#categories" class="md:block hidden"><button class="border border-black rounded-lg py-1 px-5 font-semibold tracking-wide shadow-lg">Back</button></a>
        <a href="#test" 
        @click="getQuestions(category,difficulty); letsPlay = !letsPlay; playAndPause()">
        <button class="border rounded-lg py-1 px-5 bg-blue-500 text-white font-bold tracking-wide shadow-lg">Lets Play</button></a>
      </div>
    </div>
</div>
    <!-- BAR -->
    <div v-show="letsPlay = !letsPlay">
      <div class="h-screen text-center">
      <div id="test" class="md:py-10 py-5  md:px-20 px-4">
            <div class="flex flex-row border rounded-full bg-stone-100 p-1 shadow-lg">
              <a href="#home"><button class="flex flex-row items-center border rounded-full p-2 bg-white" @click="letsPlay = !letsPlay"><RiArrowLeftSLine/>Back</button></a>
              <div class="flex">
                <audio ref="inputMusic" :src="defaultMusic" id="startMusic-001"></audio>
                <button @click="playAndPause" v-if="isMusicPlay == true"><BiVolumeUpFill/></button> 
                <button @click="playAndPause" v-else="isMusicPlay == false"><BiVolumeMuteFill/></button> 
              </div>
            </div>
      </div>

      <!-- question -->

      <div class="flex justify-center">
            <div class="md:w-2/4 m-9 bg-white border rounded-xl shadow-xl" v-for="(q, index) in questions" v-show="currentQuestion === index+1">
              <p class="font-serif mt-6 text-stone-400">Question {{ currentQuestion }} / {{ questions.length }} </p>
              <h1 class="text-xl font-semibold m-10 " >{{ q.question }}</h1>
              <p>correct : {{ q.correctAnswer }}</p> 
            </div>
      </div> 
            <!-- choices -->    
      <div class="grid md:grid-cols-2 grid-cols-1 justify-center mx-10 gap-5">
        <div v-for="choice in getChoices">
          <div class="bg-white border rounded-xl shadow-xl py-3 hover:bg-slate-200" @click="ans = choice; nextQuestion(); checkAns()">
            <p>{{ choice }}</p>                  
          </div>
        </div>

        <p>ans : {{ ans }}</p>   
      </div>
    </div>
  </div>


</template>
 
<style scoped>


</style>