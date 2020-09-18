<template>
  <div class="main-page">
    <div v-if="this.submitted">
      <p class="results">Results</p>
      <p >You got {{this.score}} / {{this.questions.length}} questions correct</p>
      <p v-bind:class="hasPassed">{{yourScore}} %</p>
    </div>
    <div >
      <div v-bind:key="question.id" v-for="(question, index) in myQuestions">
        <QuestionCard
          v-bind:rightAnswer="question.correctAnswer"
          v-bind:status="question.status"
          v-bind:index="index"
          v-bind:question="question"
          v-bind:answers="answers"
          v-bind:comKey="comKey"
          v-on:updateAnswer="chooseAnswer"
        />
      </div>
      <button v-if="!this.submitted" @click="handleSubmit" class="submit-btn">Submit</button>
      <input v-else v-on:click="restartQuiz" class="reload-btn" value="Reload" type="button" />
    </div>
    <div
      v-if="unfinished"
      class="error-msg"
    >Answer all questions before submitting. Unanswered questions are displayed in yellow.</div>
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
      unfinished: false,
      status: undefined,
      answer: "",
      correctAnswers: [...this.questions.map((q) => q.correctAnswer)],
      score: 0,
      submitted: false,
      myQuestions: this.questions,
      comKey: 0,
    };
  },
  props: ["questions"],
  computed: {
    yourScore() {
      return (this.score / this.myQuestions.length) * 100 
    },
    hasPassed() {
     return this.score > 6 ?'pass' : 'fail'
    }
  },
  methods: {
    handleSubmit() {
      // e.preventDefault();
      //adds answer object with a status of answered
      this.updateAnsweredStatus();
      if (this.answers.length < this.questions.length) {
        //set the questions that aren't answered to "no-answer"
        this.setUnansweredQuestions();
      } else {
        //sort the answers
        const answers = this.answers.sort((a, b) => {
          return a.questionId - b.questionId;
        });
        //check if the questions are correct or incorrect
        //increment or decrement the score accordingly
        answers.forEach((answer, index) => {
          if (answer.answer === this.correctAnswers[index]) {
            this.myQuestions[index].status = "correct";
            this.score += 1;
          } else {
            this.myQuestions[index].status = "incorrect";
          }
        });
        //submit and finish the quiz
        this.submitted = true;
        this.unfinished = false;
      }
    },
    chooseAnswer(answer) {
      // If the questionId already exists in the answers array, replace the answer
      //otherwise, add the answer to the array
      //prevents answers array from having two answers for the same question
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
    },
    updateAnsweredStatus() {
      //adds answer object with a status of "answered"
      this.answers.forEach((answer) => {
        let qAnswered = this.myQuestions.find(
          (q) => q.qId === answer.questionId
        );
        let index = this.myQuestions.indexOf(qAnswered);
        this.myQuestions = [
          ...this.myQuestions.slice(0, index), //copy everything before
          { ...qAnswered, status: "answered" }, // add the new question with new status
          ...this.myQuestions.slice(index + 1), //copy everything after
        ];
      });
    },
    setUnansweredQuestions() {
      //if any questions are not answered, set their status to no-answer
      //set submitted to false to render error message
      this.unfinished = true;
      this.myQuestions.forEach((q) => {
        if (q.status !== "answered") {
          q.status = "no-answer";
          this.submitted = false;
        }
      });
      //add to comKey to ensure component rerenders
      this.comKey += 1;
    },
    restartQuiz() {
      //reload the window if the quiz is submitted
      if (this.submitted) {
        window.location.reload();
      }
    },
  },
};
</script>

<style scoped>
.submit-btn {
  padding: 0.5em;
  font-size: 1em;
  color: white;
  border: 1px solid black;
  border-radius: 5px;
  background-color: green;
  margin-bottom: 2em;
}
.reload-btn {
  padding: 0.5em;
  font-size: 1em;
  color: white;
  border: 1px solid black;
  border-radius: 5px;
  background-color: green;
  margin-bottom: 2em;
}
.error-msg {
  color: red;
  padding-bottom: 3em;
}
.main-page {
  text-align: center;
  width: 80%;
  margin: auto;
}
.results {
  text-decoration: underline;
  font-size: 1em;
}
.fail {
  font-weight: 700;
  color: red;
}
.pass {
  font-weight: 700;
  color: green;
}
@media all and (max-width: 550px) and (min-width: 301px){
  .main-page{
    width: 100%;
  }
}
@media all and (max-width: 300px){
  *{
    font-size: .65em;
  }
}
</style>