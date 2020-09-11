<template>
  <div class="question-card">
    {{ question.id}}.
    {{ question.question}}
    <!-- :key="`${ key }-${ index }`" -->
    <!-- v-bind:key="index" -->
    <!-- <div class="optionals" v-for="(key, index) in question.options" v-bind:key="index"> -->
    <!-- <input type="radio" :value="index"/> -->
    <!-- {{ key }} -->
    <!-- @change="selectWinner(game.id, game.pollanswers[indexy].id)" -->
    <div v-for="(answers, index) in question.options" v-bind:key="answers.id">
      <input type="radio" :value="question.options[index].id" v-model="question.id" />
      <span>{{answers.id}} || {{index}} || {{key }}</span>
      <label for="one">{{question.options[index].answer}}</label>
      <br />
    </div>
  </div>
</template>

<script>
export default {
  name: "QuestionCard",
  props: ["question"],
};
</script>

<style scoped>
</style>