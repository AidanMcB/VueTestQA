<template>
  <div>
    <div class="results">
      <!-- conditionally render quiz results -->
    </div>
    <form @submit="handleSubmit">
      <div v-bind:key="question.id" v-for="(question, index) in questions">
        <QuestionCard
          v-bind:index="index"
          v-bind:question="question"
          v-on:addAnswer="chooseAnswer"
        />
        <!-- v-on:add-answer="$emit('add-answer', question.options[index].answer)" -->
      </div>
      <input type="submit" value="Submit" class="btn" />
    </form>
  </div>
</template>

<script>
import QuestionCard from "./QuestionCard";
export default {
  name: "QuestionsContainer",
  components: {
    QuestionCard,
  },
  // mounted() {
  //   this.correctAnswers = this.questions.map(q => q.correctAnswer)
  // },
  data() {
    return {
      answers: [],
      answer: "",
      correctAnswers: [...this.questions.map(q => q.correctAnswer)],
      score: 0,
    };
  },
  props: ["questions"],
  methods: {
    handleSubmit(e, answer) {
      e.preventDefault();
      console.log("here 2:",e, answer)
      this.answers.forEach( (answer, index ) => {
        if(answer.answer === this.correctAnswers[index]){
          answer.correct = true
          console.log(answer.correct)
        }else{
            answer.correct = false
          console.log(answer.correct)
        }
      })

    },
    chooseAnswer(answer) {
        // If the questionId already exists in the answers array, replace the answer
        //otherwise, add the answer to the array 
        if(this.answers.some(ans => ans.questionId === answer.questionId)){
            let double = this.answers.find( a => a.questionId == answer.questionId)
            let index = this.answers.indexOf(double)
            this.answers = [
              ...this.answers.slice(0, index), //copy everything before answer
              answer,                          // add the new answer 
              ...this.answers.slice(index +1) //copy everything after answer
            ]
        }else{
          //add new answer for question
          this.answers = [...this.answers, answer];
        }
    },
  },
};
</script>

<style scoped>
</style>