<script >
// import data from './data/database.json'
// get-Data
// function getQuestion(category, difficulty) {
//   fetch(
//       `https://the-trivia-api.com/api/questions?categories=${category}&limit=10&region=TH&difficulty=${difficulty}`
//   )
//       .then((response) => response.json())
//       .then((data) => (question.value = data))
//       .catch((error) => console.log(error))
//   return question
// }
export default {
  data() {
    return {
      currentQuestion: 1,
      score: 0,
      questions: [
        {
          question: 'What is the capital of France?',
          choices: ['London', 'Paris', 'Berlin', 'Rome'],
          answer: 1
        },
        {
          question: 'What is the largest ocean in the world?',
          choices: ['Atlantic Ocean', 'Indian Ocean', 'Southern Ocean', 'Pacific Ocean'],
          answer: 3
        },
        {
          question: 'What is the tallest mammal in the world?',
          choices: ['Giraffe', 'Elephant', 'Hippopotamus', 'Rhinoceros'],
          answer: 0
        }
      ]
    }
  },
  methods: {
    checkAnswer(index) {
      if (index === this.questions[this.currentQuestion].answer) {
        this.score++
      }
      this.currentQuestion++
    }
  }
}

</script>

<template>
  <div className=" min-h-screen min-w-full flex flex-col items-center bg-gray-400">

    <!-- nav-Bar -->
    <div className=" flex flex-row bg-slate-200 w-5/6 h-10  mt-6 items-center justify-between rounded-full">

      <button className=" border-solid border
            h-8 w-16
          bg-white rounded-full ml-2 hover:bg-slate-400"
      > &#60;&nbsp;Back
      </button>

      <p className=" border-solid border
            h-8 w-24 mr-8 text-center
        bg-green-400 rounded-full "
      >Score:<span>{{ score }}</span>
      </p>

      <button className=" border-solid border
            h-9 w-9
          bg-white rounded-full mr-2"
      >DM
      </button>
    </div>


    <!-- content -->
    <div className="grid grid-col-1 w-4/6 ">
      <!-- Question -->
      <div className=" flex flex-col bg-white w-full h-auto mt-10 items-center justify-center rounded-3xl py-7 px-3">
        <div className="text-center">

          <p className=" mt-3 text-gray-800">Question {{ currentQuestion }}/10</p>

          <h2 className=" font-semibold break-all m-3 text-4xl items-center "
          >{{ questions[currentQuestion].question }}
          </h2>
        </div>
      </div>
      <!-- Options -->
      <div className=" grid grid-cols-1 gap-3 bg-slate-300 mt-5 py-5 px-10 rounded-3xl w-full  lg:grid-cols-2">
        <div v-for="(choice, index) in questions[currentQuestion].choices" :key="index">
          <button className="bg-white rounded-3xl break-all py-3 px-2 w-full lg:px-10 hover:bg-slate-400" @click="checkAnswer(index)">
            {{ choice }}
          </button>
        </div>
      </div>


      <!-- reload-Button  -->
      <div className="flex mt-7 justify-center">
        <button className=" border-solid border h-9 w-9 bg-white rounded-full mr-2">RL</button>
      </div>


      <!-- REF -->
      <div className="flex bg-gray-600 rounded-3xl w-full justify-center p-4 mt-10">
        <p>All questions are from <a className=" hover:text-orange-100" href="https://the-trivia-api.com/docs/#search">The
          Trivia API</a></p>
      </div>
    </div>


    <!-- result-score -->
    <!-- <div class=" bg-black bg-opacity-50  absolute w-full min-h-screen flex flex-col items-center">
        <div class=" flex flex-col w-3/6 h-full bg-white text-center mt-40 rounded-3xl p-2 items-center">
          <p class=" m-3 break-all">You've finished all quizes. Here's your score </p> -->
    <!-- circle-score -->
    <!-- <div class=" bg-gray-300 rounded-full w-36 h-36 pt-9 mt-10">
        <p class=" text-4xl font-extrabold">10</p>
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
