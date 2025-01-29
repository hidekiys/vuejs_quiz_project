<template>
  <div>

    <ScoreBoard :winCount="this.winCount" :loseCount="this.loseCount"/>
    <template v-if="this.question">
      <h1 v-html="this.question">
      </h1>
      <template v-for="(answer, index) in this.answers" :key="index">
        <input type="radio" name="options" :value="answer" :id="answer" v-model="this.chosenAnswer"
        :disabled="this.answerSubmited"
        >
        <label v-html="answer" :for="answer"></label>
        <br>
      </template>
      <br>
      <button class="send" type="button" v-if="!this.answerSubmited" @click="submitAnswer()">Enviar</button>
      <button class="send" type="button" v-if="this.answerSubmited" @click="getNewQuestion()">Próxima Questão</button>
      <template v-if="this.answerSubmited">
        <section class="result">
          <h4>
            {{ result }}
          </h4>

        </section>

      </template>


    </template>

  </div>
</template>

<script>


import ScoreBoard from './components/ScoreBoard.vue';
export default {
  name: 'App',
  components: {
    ScoreBoard
  },

  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosenAnswer: undefined,
      answerSubmited: false,
      result: '',
      winCount: 0,
      loseCount: 0
    }
  },

    computed: {
      answers() {
        var answers = [...this.incorrectAnswers];
        answers.splice(Math.round(Math.random()*answers.length), 0, this.correctAnswer)
        return answers;
      }
    },

    methods:{

      submitAnswer() {
        if(this.chosenAnswer){
          this.answerSubmited = true
          if(this.chosenAnswer == this.correctAnswer){
            this.result = '✅ Parabéns! Você escolheu a alternativa correta!'
            this.winCount++;
          }else{
            this.result = '❌ Você escolheu a alternativa incorreta! A alternativa correta era '+this.correctAnswer
            this.loseCount++;
          }
        }else{
          alert('Selecione uma opção!')
        }
      },
      getNewQuestion() {
        this.axios
        .get("https://opentdb.com/api.php?amount=1&category=10&difficulty=medium")
        .then((response)=>{
          this.question = response.data.results[0].question
          this.incorrectAnswers = response.data.results[0].incorrect_answers
          this.correctAnswer = response.data.results[0].correct_answer
          this.answerSubmited = false
          this.chosenAnswer = undefined
        })
        .catch((err)=>{
          console.log(err)
    })
      }

    },
  created() {
    this.getNewQuestion()
  },
}

</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

  input[type=radio] {
    margin: 12px 4px;
  }

  .send {
    margin-top: 12px;
    height: 35px;
    min-width: 120px;
    padding: 0 16px;
    background-color: cornflowerblue;
    border: 1px solid cornflowerblue;
    cursor: pointer;
    color: white;
  }

  .send:hover {
    background-color:#2c3e50;
    border: #2c3e50;
  }
}

</style>
