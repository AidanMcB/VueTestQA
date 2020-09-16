<template>
  <div class="main-page">
    <div>
      <p class="results" v-if="this.submitted">Results</p>
      <p v-if="this.submitted">You got {{this.score}} / 10 questions correct</p>
      <p class="percent" v-if="this.submitted">{{ (this.score / this.myQuestions.length) * 100 }} %</p>
    </div>
    <form @submit="handleSubmit">
      <div v-bind:key="question.id" v-for="(question, index) in myQuestions">
        <QuestionCard
          v-bind:rightAnswer="question.correctAnswer"
          v-bind:status="question.status"
          v-bind:index="index"
          v-bind:question="question"
          v-bind:answers="answers"
          v-bind:comKey="comKey"
          v-on:updateAnswer="chooseAnswer"
          v-on:forceRerender="testIt"
        />
        <!-- v-on:add-answer="$emit('add-answer', question.options[index].answer)" -->
      </div>
      <input v-if="!this.submitted" type="submit" value="Submit" class="submit-btn" />
      <input v-else v-on:click="restartQuiz" class="reload-btn" value="Reload" type="button" />
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
      missedAnswer: "",
      comKey: 0,
    };
  },
  props: ["questions"],
  updated() {
    // console.log("HERE HERE");
  },
  methods: {
    testIt(forceRerender){
      // console.log("test it", forceRerender)
      forceRerender()
    },
    handleSubmit(e) {
      e.preventDefault();
      //iterate over answers array
      this.answers.forEach((answer) => {
        //find the question in the questions array
        let qAnswered = this.myQuestions.find(
          (q) => q.qId === answer.questionId
        );
        //get the index of that question that
        let index = this.myQuestions.indexOf(qAnswered);
        //update myQuestion to set status to answered on that question
        this.myQuestions = [
          ...this.myQuestions.slice(0, index), //copy everything before
          { ...qAnswered, status: "answered" }, // add the new question with new status
          ...this.myQuestions.slice(index + 1), //copy everything after
        ];
      });

      // if there are less than 10 answers
      if (this.answers.length < this.questions.length) {
        //set the questions that aren't answered to "no-answer"
        //filter questions to an array that aren't answered
        this.myQuestions.forEach((q) => {
          if (q.status !== "answered") {
            q.status = "no-answer";
            this.unifnished = true;
            this.submitted = false;
          }
        });
        this.comKey += 1
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
        this.submitted = true;
        this.finished = true;
        //if questions are unanswered, display error message
        //do not submit the form
      }
    },
    chooseAnswer(answer) {
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
    },
    restartQuiz() {
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
}
.reload-btn {
  padding: 0.5em;
  font-size: 1em;
  color: white;
  border: 1px solid black;
  border-radius: 5px;
  background-color: green;
}
.error-msg {
  color: red;
}
.main-page {
  text-align: center;
}
.results {
  text-decoration: underline;
  font-size: 1em;
}
.percent {
  font-weight: 700;
  color: red;
}
</style>