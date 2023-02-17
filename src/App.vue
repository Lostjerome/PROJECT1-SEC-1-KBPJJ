<script setup>
import { ref } from "vue"
const questionArr = ref([])
const currentTG = ref(0)
const score = ref(0)


// get-Data
  fetch('https://the-trivia-api.com/api/questions?categories=music&limit=10&difficulty=medium')
      .then(response =>response.json())
      .then(data =>(questionArr.value = data))
      .catch(error => (error))
  ;
      
function shuffle(array) {
  return array?.sort(() => Math.random() - 0.5)
}

      function randomChoice(choiceTarget){
            const correctchoice = []
            const incorrectChoiceArr = Object.values(choiceTarget.incorrectAnswers)
            correctchoice.push(choiceTarget.correctAnswer)
            const choice = [...correctchoice,...incorrectChoiceArr]
        return choice
      }









</script>

<template>
 <div class=" min-h-screen min-w-full flex flex-col items-center bg-gray-400" >

   <!-- nav-Bar -->
    <div class=" flex flex-row bg-slate-200 w-5/6 h-10  mt-6 items-center justify-between rounded-full">
      
        <button  class=" border-solid border
            h-8 w-16
          bg-white rounded-full ml-2 hover:bg-slate-400" 
              > &#60;&nbsp;Back
        </button>

        <p class=" border-solid border
            h-8 w-24 mr-8 text-center
        bg-green-400 rounded-full "
              >Score:<span>&nbsp;{{ score }}</span>
        </p>

        <button class=" border-solid border
            h-9 w-9 
          bg-white rounded-full mr-2"
              >DM
        </button>
    </div>


    <!-- content -->
    <div class="grid grid-col-1 w-4/6 ">
      <!-- Question -->
      <div class=" flex flex-col bg-white w-full h-auto mt-10 items-center justify-center rounded-3xl py-7 px-3">
          <div class="text-center">

            <p class=" mt-3 text-gray-800">Question {{currentTG+1}}/10</p>

            <h2 class=" font-semibold break-all m-3 text-4xl items-center" 
                  > {{ questionArr[currentTG].question }}
            </h2>
          </div>
      </div>
       <!-- Choice -->
        <div class="grid grid-cols-1 gap-3 bg-slate-300 mt-5 py-5 px-10 rounded-3xl w-full lg:grid-cols-2">
            <div v-for =" (getC,index) in randomChoice(questionArr[currentTG]) " :key="index" id="getC"
            @click="userClick = getC; userClick === questionArr[currentTG].correctAnswer ? score++ :''; ++currentTG ; 
                 ">
                
                <button class="bg-white rounded-3xl break-all py-3 px-2 w-full lg:px-10 hover:bg-slate-400"
                        >{{  getC  }}
                </button>
            </div>
        </div>
      
        
        <!-- reload-Button  -->
        <div class="flex mt-7 justify-center">
          <button class=" border-solid border h-9 w-9 bg-white rounded-full mr-2">RL</button>
        </div>


        <!-- REF -->
        <div class="flex bg-gray-600 rounded-3xl w-full justify-center p-4 mt-10">
            <p>All questions are from <a class=" hover:text-orange-100" href="https://the-trivia-api.com/docs/#search">The Trivia API</a></p>
        </div>
    </div>    






    <!-- result-score -->
    <!-- <div class=" bg-black bg-opacity-50  absolute w-full min-h-screen flex flex-col items-center">
        <div class=" flex flex-col w-3/6 h-full bg-white text-center mt-40 rounded-3xl p-2 items-center">
          <p class=" m-3 break-all">You've finished all quizes. Here's your score </p> -->
          <!-- circle-score -->
          <!-- <div class=" bg-gray-300 rounded-full w-36 h-36 pt-9 mt-10">
              <p class=" text-4xl font-extrabold">{{ score }}</p>
              <p class=" font-medium">Out of 10</p>
          </div> -->
          <!-- button-action -->
          <!-- <div class=" flex flex-col gap-4 font-bold mt-5 py-4 w-3/4 text-center">
              <button class=" bg-blue-500 text-white rounded-full w-full py-2 ">Play again</button>
              <button class=" bg-gray-100 rounded-full p-2 py-2">Home</button>
          </div>
        </div>
    </div> -->










</div>
</template>

<style scoped>
</style>
