<template>
  <div class="quiz-wrapper d-flex justify-content-center align-items-center py-5">
    <b-card class="shadow-lg quiz-card p-4">

      <!-- Question -->
      <h4 class="question-text">{{ currentQuestion.question }}</h4>

      <!-- Answers -->
      <div class="mt-4">
        <b-list-group>
          <b-list-group-item
            v-for="(answer, index) in answers"
            :key="index"
            @click="selectAnswer(index)"
            :class="['answer-item', answerClass(index)]"
            button
          >
            {{ answer }}
          </b-list-group-item>
        </b-list-group>
      </div>

      <!-- Buttons -->
      <div class="mt-4 d-flex gap-3 justify-content-end">
        <b-button 
          variant="primary"
          class="px-4"
          @click="submitAnswer"
          :disabled="selectedIndex === null || answered"
        >
          Submit
        </b-button>

        <b-button class="px-4" variant="success" @click="next">
          Next →
        </b-button>
      </div>

    </b-card>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  name: 'QuestionBox',
  data (){
    return {
    selectedIndex: null,
    shuffledAnswers:[],
    correctIndex : null,
    answered: false
    }
  },
  
  props: {
      currentQuestion:Object,
      next:Function,
      increment: Function
  },

   methods:{
    submitAnswer(){
      let isCorrect = false
      if( this.selectedIndex === this.correctIndex){
        isCorrect = true
      }
      this.answered = true
      this.increment(isCorrect)
    },
    selectAnswer(index){
      this.selectedIndex = index
    },
    shuffleAnswers(){
      let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },

    answerClass (index) {
      let answerClass = ''
      if(!this.answered && this.selectedIndex === index)
      {
        answerClass = 'selected'
      }
      else if (this.answered && this.correctIndex === index){
        answerClass = 'correct'

      }
      else if (this.answered && this.correctIndex !== index && this.selectedIndex === index) {
        answerClass = 'in-correct'
      }
      return answerClass
    }
  },

   computed:{
     answers(){
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      return answers
     }
  },

  watch:{
    currentQuestion: {
      immediate: true,
      handler()
      {
        this.answered = false
        this.selectedIndex = null
        this.shuffleAnswers()
      }
    }
  }
}
</script>
<!-- <style scoped>
.b-list-group-item:hover{
  cursor: pointer;
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
}

.in-correct {
  background-color: red;
}


</style> -->

<style scoped>
.quiz-wrapper {
  background: #f4f7fb;
  min-height: 90vh;
}

.quiz-card {
  max-width: 600px;
  width: 100%;
  border-radius: 16px;
  background: white;
}

.question-text {
  text-align: center;
  font-weight: 600;
}

.answer-item {
  cursor: pointer;
  transition: background 0.3s, transform 0.2s;
  border-radius: 6px;
  margin-bottom: 10px;
}

.answer-item:hover {
  background: #e9effd;
  transform: scale(1.01);
}
</style>

