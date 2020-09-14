<template>
  <div class="question-card">
    <p v-if="correct == true">Correct</p>
    <p v-else-if="correct == false">Incorrect</p>
      {{ index + 1}}.
      {{ question.question}}
      <div class="answers" v-for="(answers, index, key) in question.options" v-bind:key="key">
        <!-- value  -->
        <input
          type="radio"
          :value="question.options[index].id"
          v-model="question.id"
          v-on:change="addAnswer(question.options[index])"
        />
        <button v-on:click="printThis(question.correct)">here</button>
        <label class="answer-choice">{{question.options[index].answer}}</label>
        <br />
      </div>
  </div>
</template>

<script>
    //  v-on:change="addAnswer(question.options[index])"
export default {
  name: "QuestionCard",
  props: ["question", "index", "correct"],
  data() {
    return {
        // questionId: '',
        // answer: '',
        // correct: undefined,
      };
  },
  methods: {
    addAnswer(answer) {
      // console.log(answer)
      // this.answer = answer
      this.$emit("addAnswer", answer);
    },
    printThis(x){
      console.log(x)
    }
  },
};
</script>

<style scoped>
.question-card {
  border: 1px solid black;
  margin: 1.25em;
  padding: 1.25em;
}
.answers {
  padding: 0.1em;
  margin-left: 1.5em;
}
.answer-choice {
  margin-left: 0.25em;
}
</style>