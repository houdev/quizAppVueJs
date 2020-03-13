<template>
  <div id="app">
    <Header
      :correctAns="correctAns"
      :total="total"
      />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <Question-Box
            v-if="Questions.length"
            :CurrentQuestion="Questions[index]"
            :next="next"
            :correctAnswersCounter="correctAnswers"
            />
        </b-col>
      </b-row>
    </b-container>
   
  </div>
</template>

<script>

import Header from './components/Header.vue';
import QuestionBox from './components/QuestionBox.vue';

export default {
  name: 'App',
  components: {
   Header,
   QuestionBox
  },
  data(){
    return {
      Questions: [],
      index:0,
      correctAns:0,
      total:0
    }
  },
  methods:{
    next(){
      this.index++
    },
    correctAnswers(correct){
      if(correct){
        this.correctAns++
      }
      this.total++
    }
  },
  mounted(){
    fetch('https://opentdb.com/api.php?amount=10&category=9&difficulty=easy&type=multiple', {
      method:'get'
      })
      .then((res) => {
        return res.json();
      })
      .then((Json)=> {
        this.Questions = Json.results;
      })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
