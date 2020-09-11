<template>
  <div class="question-card">
    {{ question.id}}.
    {{ question.question}}
    <div v-for="(answers, index, key) in question.options" v-bind:key="index">
      <input type="radio" :value="question.options[index].id" v-model="question.id"/>
      <span > {{ key }}</span>
      <label >{{question.options[index].answer}}</label>
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