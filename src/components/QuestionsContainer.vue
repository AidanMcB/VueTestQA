<template>
  <div>
    <div class="results">
      <!-- conditionally render quiz results -->
    </div>
    <form @submit="handleSubmit">
      <div v-bind:key="question.id" v-for="question in questions">
        <QuestionCard
          v-bind:question="question"
          v-on:addAnswer="chooseAnswer($event)"
        />
        <!-- v-on:add-answer="$emit('add-answer', question.options[index].answer)" -->
      </div>
      <input type="submit" value="Submit" class="btn" />
    </form>
    <button @click="printThis('x')">here</button>
  </div>
</template>

<script>
import QuestionCard from "./QuestionCard";
export default {
  name: "QuestionsContainer",
  components: {
    QuestionCard,
  },
  data() {
    return {
      answers: [],
      answer: "",
    };
  },
  props: ["questions"],
  methods: {
    handleSubmit(e) {
      e.preventDefault();
      console.log("quiz submitted");
    },
    chooseAnswer(answerObj) {
        console.log(answerObj)
        // If the questionId already exists in the answers array, replace the answer
        //otherwise, add the answer to the array 
        // const answerObj = {
        //     answer: newAnswer.answer,
        //     id: newAnswer.id,
        //     questionId: newAnswer.questionId
        // }
        if(this.answers.some(ans => ans.questionId === answerObj.questionId)){
            console.log("same questionId exists")
            //filter 
            this.answers = this.answers.map(ans => this.answers.find(answer => ans.questionId === answer.questionId) || ans);
        }else{
      this.answers = [...this.answers, answerObj];
      console.log(this.answers)
        }
    },
  },
};
</script>

<style scoped>
</style>