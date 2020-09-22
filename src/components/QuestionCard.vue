<template>
  <div v-bind:class="cardStyle(question)" :key="componentKey">
    <p class="question-text">  {{ index + 1}}. {{ question.question}}</p>
    <div
      v-for="(answers, index) in question.options"
      v-bind:key="answers.id"
      v-bind:class="{correctAnswer: (status == 'incorrect' && question.options[index].answer == question.correctAnswer)}"
    >
      <div class='answer-choice'>
        <input
          type="radio"
          :id="question.options[index].id"
          :value="question.options[index].id"
          v-model="question.id"
          v-on:change="updateAnswer(question.options[index])"
        />
        <label :for="question.options[index].id" class="answers">{{question.options[index].answer}}</label>
      </div>
    </div>
    <p class="cor-tag" v-if="status == 'correct'">Correct</p>
    <p class="wrong-tag" v-else-if="status == 'incorrect'">Wrong</p>
  </div>
</template>

<script>
export default {
  name: "QuestionCard",
  props: [
    "question",
    "index",
    "status",
    "answer",
    "answers",
    "rightAnswer",
    "comKey",
  ],
  data() {
    return {
      componentKey: 0,
    };
  },
  //when this component is updated, run the dynamc method passing it the question object
  updated() {
    this.cardStyle(this.question);
  },
  computed: {
    //when the quiz is submitted, add style to the right answer that was missed
    showAnswer: function (ans) {
      let rightAnswer = "undecorated";
      if (ans.status == "incorrect" && ans.rightAnswer == this.rightAnswer) {
        rightAnswer = "right-answer";
      }
      return rightAnswer;
    },
  },
  methods: {
    //depending on the questions status after submission, style the q card
    cardStyle(question) {
      let outerCardClass = "";
      if (question.status == "correct") {
        outerCardClass = "correct";
      } else if (question.status == "incorrect") {
        outerCardClass = "incorrect";
      } else if (question.status == "no-answer") {
        outerCardClass = "no-answer";
      } else {
        outerCardClass = "qCard";
      }
      return outerCardClass;
    },
    //emit the updateAnswer event to the parent component with the answer object
    updateAnswer(answer) {
      this.myAnswer = answer;
      this.$emit("updateAnswer", answer);
    },
  },
};
</script>

<style scoped>
.undecorated {
  text-decoration: none;
}
.correctAnswer {
  color: white;
  background-color:limegreen;
  border: 4px solid green;
  border-radius: 10px;
  padding: .15em;
  width: 75%;
}
.no-answer {
  background-color: white;
  text-align: left;
  border: .25em solid yellow;
  border-radius: 10px;
  box-shadow: 2px 2px 2px 2px gray;
  margin: 2.25em;
  padding: 1.25em;
}
.qCard {
  background-color: white;
  text-align: left;
  border: .1em solid black;
  border-radius: 10px;
  box-shadow: 1px 1px 1px 1px gray;
  margin: 2.25em;
  padding: 1.25em;
}
.correct {
  background-color: white;
  text-align: left;
  padding: 0.1em;
  margin-left: 1.5em;
  border: .25em solid green;
  border-radius: 10px;
  margin: 2.25em;
  padding: 1.25em;
  padding-bottom: .25em;
}
.incorrect {
  background-color: white;
  text-align: left;
  padding: 0.1em;
  margin-left: 1.5em;
  border: .23em solid red;
  border-radius: 10px;
  margin: 2.25em;
  padding: 1.25em;
  padding-bottom: .25em;
}
.options {
  padding: 0.1em;
  margin-left: 2em;
}
.answers {
  margin-left: 1em;
}
.answer-choice {
  padding: .25em;
  margin-left: 1.25em;
}
.cor-tag {
  vertical-align: bottom;
  text-align: right;
  color: green;
}
.wrong-tag {
  vertical-align: bottom;
  text-align: right;
  color: red;
}
.question-text{
  margin-bottom: .75em;
}
</style>