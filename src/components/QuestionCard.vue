<template>
  <div class="question-card">
    {{ question.id}}.
    {{ question.question}}
    <ul class="options">
        <li v-bind:key="option.index" v-for="option in options">{{option}}</li>
    </ul>
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