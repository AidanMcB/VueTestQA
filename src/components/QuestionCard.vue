<template>
  <div v-bind:class="dynamic">
    {{ index + 1}}.
    {{ question.question}}
    <div  v-for="(answers, index, key) in question.options" v-bind:key="key" class="options">
      <!-- value  -->
      <div v-bind:class="showAnswer(question)" >
      <input 
        type="radio"
        :value="question.options[index].id"
        v-model="question.id"
        v-on:change="addAnswer(question.options[index])"
      />
      <label class="answers">{{question.options[index].answer}}</label>
      </div>
      <br />
    </div>
    <p class="cor-tag" v-if="status == 'correct'">Correct</p>
    <p class="wrong-tag" v-else-if="status == 'incorrect'">Incorrect</p>
  </div>
</template>

<script>
//  v-on:change="addAnswer(question.options[index])"
export default {
  name: "QuestionCard",
  props: ["question", "index", "status", "answer"],
  data() {
    return {
      // questionId: '',
      // answer: '',
      // correct: undefined,
    };
  },
  computed: {
    dynamic: function (ans) {
      let outerCardClass = "";

      if (ans.status == "correct") {
        outerCardClass = "correct";
      } else if (ans.status == "incorrect") {
        outerCardClass = "incorrect";
      } else if (ans.status == "no-answer") {
        outerCardClass = "no-answer";
      } else {
        outerCardClass = "qCard";
      }
      // console.log(ans.correct)
      return outerCardClass;
    },
    showAnswer: function (ans, question) {
      console.log(ans, question)
      let rightAnswer = "undecorated";
      if (ans.status == "incorrect"){
        console.log("true")
        rightAnswer = "right-answer"
      }
      return rightAnswer;
  

    }
  },
  methods: {
    addAnswer(answer) {
      this.$emit("addAnswer", answer);
    },
    printThis(x) {
      console.log(x);
    },
  },
};
</script>

<style scoped>
.undecorated{
  text-decoration: none;
}
.right-answer{
  background-color: green;
  color: green;
}
.no-answer {
  text-align: left;
  border: 3px solid yellow;
  border-radius: 10px;
  box-shadow: 1px 1px 1px 1px gray;
  margin: 1.25em;
  padding: 1.25em;
}
.qCard {
  text-align: left;
  border: 1px solid black;
  border-radius: 10px;
  box-shadow: 1px 1px 1px 1px gray;
  margin: 1.25em;
  padding: 1.25em;
}
.correct {
  text-align: left;
  padding: 0.1em;
  margin-left: 1.5em;
  border: 3px solid green;
  border-radius: 10px;
  box-shadow: 1px 1px 1px 1px gray;
  margin: 1.25em;
  padding: 1.25em;
}
.incorrect {
  text-align: left;
  padding: 0.1em;
  margin-left: 1.5em;
  border: 2px solid red;
  border-radius: 10px;
  box-shadow: 1px 1px 1px 1px gray;
  margin: 1.25em;
  padding: 1.25em;
}
.options {
  padding: 0.1em;
  margin-left: 1em;
}
.answers {
  margin-left: 1em;
}
.answer-choice {
  margin-left: 0.25em;
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
</style>