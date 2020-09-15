<template>
  <div class="main-page">
    <div>
      <p class="results" v-if="this.submitted">Results</p>
      <p v-if="this.submitted">You got {{this.score}} / 10 questions correct</p>
      <p class="percent" v-if="this.submitted">{{ (this.score / this.myQuestions.length) * 100 }} %</p>
    </div>
    <form @submit="handleSubmit">
      <div v-bind:key="question.id" v-for="(question, index) in questions">
        <QuestionCard
          v-bind:status="question.status"
          v-bind:index="index"
          v-bind:question="question"
          v-on:addAnswer="updateAnswer"
        />
        <!-- v-on:add-answer="$emit('add-answer', question.options[index].answer)" -->
      </div>
      <input type="submit" value="Submit" class="btn" />
    </form>
    <div
      v-if="unfinished"
      class="error-msg"
    >Answer all questions before submitting. Unanswered questions are displayed in yellow</div>
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
      unfinished: false,
      status: undefined,
      answer: "",
      correctAnswers: [...this.questions.map((q) => q.correctAnswer)],
      score: 0,
      submitted: false,
      myQuestions: this.questions,
      missedAnswer: ""
    };
  },
  props: ["questions"],
  methods: {
    handleSubmit(e) {
      e.preventDefault();
      //sort the answers
      const answers = this.answers.sort((a, b) => {
        return a.questionId - b.questionId;
      });
      //check that all questions have beeen answered
      if (this.myQuestions.every((q) => q.status == "answered")) {
        //check if the questions are correct or incorrect
        //increment or decrement the score accordingly
        answers.forEach((answer, index) => {
          if (answer.answer === this.correctAnswers[index]) {
            this.questions[index].status = "correct";
            this.score += 1;
          } else {
            this.questions[index].status = "incorrect";
          }
        });
        this.submitted = true;
      } else {
        //if questions are unanswered, display error message
        //do not submit the form
        this.myQuestions.forEach( q => {
          if(q.status == "undefined"){
            q.status = "no-answer"
          }
        })
        console.log(this.myQuestions)
        this.submitted = false;
        this.unfinished = true;
      }
    },
    updateAnswer(answer) {
      // If the questionId already exists in the answers array, replace the answer
      //otherwise, add the answer to the array
      if (this.answers.some((ans) => ans.questionId === answer.questionId)) {
        let double = this.answers.find(
          (a) => a.questionId == answer.questionId
        );
        let index = this.answers.indexOf(double);
        this.answers = [
          ...this.answers.slice(0, index), //copy everything before answer
          answer, // add the new answer
          ...this.answers.slice(index + 1), //copy everything after answer
        ];
      } else {
        //add new answer for question
        this.answers = [...this.answers, answer];
      }
      //answered the question
      let qAnswered = this.myQuestions.find((q) => q.qId === answer.questionId);
      qAnswered.status == "answered";
      let index = this.myQuestions.indexOf(qAnswered);
      this.myQuestions = [
        ...this.myQuestions.slice(0, index), //copy everything before
        { ...qAnswered, status: "answered" }, // add the new question with new status
        ...this.myQuestions.slice(index + 1), //copy everything after
      ];
    },
  },
};
</script>

<style scoped>
.btn {
  padding: 0.25em;
  background-color: green;
}
.error-msg{
  color: red;
}
.main-page {
  text-align: center;
}
.results{
  text-decoration: underline;
  font-size: 1em;
}
</style>