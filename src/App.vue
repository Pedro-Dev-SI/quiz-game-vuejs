<template>
  <div>

    <ScoreBoard :playerScore="this.playerScore" :machineScore="this.machineScore"/>

    <template v-if="this.answers">
      <h1 v-html="this.question"></h1>

      <template v-for="(answer, index) in this.answers" :key="index">
        <input 
        type="radio" 
        name="options" 
        :value="answer" 
        v-model="this.checkedAnswer" 
        :disabled="this.answerSubmitted"
        />
        <label v-html="answer"></label><br>
      </template>

      <button v-if="!this.answerSubmitted" @click="submitAnswer()" class="send" type="button">Send</button>

      <section v-if="this.answerSubmitted" class="result">
        <h4 v-if="checkedAnswer == this.correctAnswer">
          &#9989; You've picked the correct answer.
        </h4>

        <h4 v-else>
          &#10060; You've picked the wrong answer. The correct is "{{this.correctAnswer}}".
        </h4>
        <button @click="this.getNewQuestion()" class="send" type="button">Next Question</button>
      </section>
      
    </template>
    
  </div>
</template>

<script>

import ScoreBoard from './components/ScoreBoard.vue'

export default {

  name: 'App',
  components:{
    ScoreBoard
  },

  data(){
    return{
      question: undefined,
      incorrectAnswer: undefined,
      correctAnswer: undefined,
      checkedAnswer: undefined,
      answerSubmitted: false,
      playerScore: 0,
      machineScore: 0,
    }
  },

  computed: {
    answers() {
      let answers = JSON.parse(JSON.stringify(this.incorrectAnswer));
      answers.splice(Math.round(Math.random() * answers.length), 0, this.correctAnswer);
      return answers;
    }
  },

  methods:{

    submitAnswer(){
      if(!this.checkedAnswer){
        alert('Please select an answer');
      }else{
        this.answerSubmitted = true;
        if(this.checkedAnswer == this.correctAnswer){
          this.playerScore++;
        }else{
          this.machineScore++;
        }
      }
    },

    getNewQuestion(){

      this.answerSubmitted = false;
      this.checkedAnswer = undefined;
      this.question = undefined;

      this.axios
      .get("https://opentdb.com/api.php?amount=1")
      .then((response) => {
      this.question = response.data.results[0].question;
      this.incorrectAnswer = response.data.results[0].incorrect_answers;
      this.correctAnswer = response.data.results[0].correct_answer;
      })
    }
  },

  //Eu quero fazer a requisição logo após de criar a aplicação
  created() {
    this.getNewQuestion();
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;
}

input[type=radio] {
  margin: 12px 4px;
}

.send{
  margin-top: 12px;
  height: 40px;
  min-width: 120px;
  padding: 0 16px;
  color: #fff;
  background-color: #1867c0;
  border: 1px solid #1867c0;
  cursor: pointer;
}
</style>
