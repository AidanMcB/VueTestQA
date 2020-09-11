<template>
  <div id="app">
    <Header />
    <h3>Quiz Title</h3>
    <QuestionsContainer v-bind:questions="questions"  v-on:add-answer-second="chooseAnswer($event)"/>
  </div>
</template>

<script> 
import QuestionsContainer from './components/QuestionsContainer'
import Header from './components/layouts/Header'
import json from './assets/questions.json'
console.log(json)
export default {
  name: 'App',
  components: {
    Header,
    QuestionsContainer,
  },
  data() {
    return {
      questions: [...json],
    }
  },
  methods: {

  }
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}
</style>
