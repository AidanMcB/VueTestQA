<template>
  <div class="question-card">
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
      <button v-on:click="printThis(question.correct)">here it</button>
        <label class="answer-choice"> {{question.options[index].answer}}</label>
      <br />
    </div>
  </div>
</template>

<script>
export default {
  name: "QuestionCard",
  props: ["question", "index"],
  data() {
    return {
      value: '123',
      correct: undefined,
    };
  },
  methods: {
    addAnswer(answer) {
        //emit event, name the event, pass data
        console.log(answer)
      this.$emit("addAnswer", answer);
    },
    printThis(x){
      console.log(x)
    }
  },
};
</script>

<style scoped>
.question-card{
  border: 1px solid black;
  margin: 1.25em; 
  padding: 1.25em;
}
.answers{
  padding: .1em;
  margin-left: 1.5em;
}
.answer-choice{
  margin-left: .25em;
}
</style>