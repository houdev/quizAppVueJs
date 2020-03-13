<template>
    <div>
      <b-jumbotron>

        <template v-slot:lead>
          {{ CurrentQuestion.question }}
        </template>

        <hr class="my-4">

        <b-list-group>
          <b-list-group-item 
            v-for="(answer, index) in answers" :key="index"
            @click="selectIndex(index)"
            :class="answerClass(index)"
            >
              {{ answer }}
          </b-list-group-item>
        </b-list-group>

        <b-button 
        variant="primary" 
          :disabled="selectdAnswer == null || answered == true" 
          @click="submitAnswer"
        >
          Submit
        </b-button>
        <b-button 
          variant="success" 
          :disabled="answered == null" 
          @click="next" 
        >
          Next
        </b-button>
      </b-jumbotron>
    </div>
</template>

<script>

import _ from 'lodash';

export default {
    name:"QuestionBox",
    props:{
        CurrentQuestion: Object,
        next: Function,
        correctAnswersCounter: Function
    },
    data(){
      return {
      selectdAnswer:null,
      correctAnswer:null,
      answered:null
      }
    },
    watch:{
      answers(){
       this.selectdAnswer = null;
       this.answered = null;
      }
    },
    methods:{
      selectIndex(index){
        this.selectdAnswer = index;
      },
      submitAnswer(){
        let isCorrect = false;

        if(this.correctAnswer == this.selectdAnswer){
          isCorrect = true;
        }
        this.correctAnswersCounter(isCorrect)

        this.answered = true;
      },
      answerClass(index){
        let answerclass = "";

        if(this.answered && this.correctAnswer === index ){
          answerclass = "correct";
        }else if(this.answered && this.selectdAnswer === index && this.correctAnswer !== index ){
          answerclass = "incorrect";
        }
        else if(this.selectdAnswer === index){
          answerclass = "selected";
        }
        
        return answerclass;

      }
    },
    computed:{
      answers(){
        let answers = this.CurrentQuestion.incorrect_answers;
            answers.push(this.CurrentQuestion.correct_answer);
        var shuffledAnswers = _.shuffle(answers);
            return shuffledAnswers;
      }
    },
    updated() {
        this.correctAnswer = this.answers.indexOf(this.CurrentQuestion.correct_answer);
    }
}
</script>

<style scoped>
.list-group{
  margin-bottom: 17px;
}
.list-group-item:hover{
  background: #EEE;
  cursor: pointer;
}
.btn{
  margin: 0 15px;
  width:30%;
}
.selected{
  background: lightblue;
  z-index: 1;
}
.correct{
  background: lightgreen;
  z-index: 3;
}
.incorrect{
   background: lightcoral;
   z-index: 2;
}
</style>