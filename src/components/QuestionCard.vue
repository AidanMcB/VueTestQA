<template>
  <div class="question-card">
    {{ question.id}}.
    {{ question.question}}
    <div v-for="(answers, index, key) in question.options" v-bind:key="index">
      <!-- value  -->
      <input
        type="radio"
        :value="question.options[index].id"
        v-model="question.id"
        v-on:change="addAnswer(question.options[index].answer)"
      />
      <span>{{ key }}</span>
      <label>{{question.options[index].answer}}</label>
      <br />
    </div>
  </div>
</template>

<script>
export default {
  name: "QuestionCard",
  props: ["question"],
  data() {
    return {
      value: '123',
    };
  },
  methods: {
    addAnswer(answer) {
        //emit event, name the event, pass data
      this.$emit("addAnswer", answer);
    },
  },
};
</script>

<style scoped>
</style>