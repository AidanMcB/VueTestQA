<template>
  <div>
    <div class="results">
      <!-- conditionally render quiz results -->
    </div>
    <form @submit="handleSubmit">
      <div v-bind:key="question.id" v-for="question in questions">
        <QuestionCard
          v-bind:question="question"
          v-on:addAnswer="chooseAnswer($event)"
        />
        <!-- v-on:add-answer="$emit('add-answer', question.options[index].answer)" -->
      </div>
      <input type="submit" value="Submit" class="btn" />
    </form>
    <button @click="printThis('x')">here</button>
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
      answer: "",
    };
  },
  props: ["questions"],
  methods: {
    handleSubmit(e) {
      e.preventDefault();
      console.log("quiz submitted");
    },
    chooseAnswer(newAnswer) {
      this.answers = [...this.answers, newAnswer];
    },
  },
};
</script>

<style scoped>
</style>