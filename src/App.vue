<template>
  <div id="app">
    <Header />
    <h3 class="title">Quiz Title</h3>
    <QuestionsContainer v-bind:questions="questions"/>
  </div>
</template>

<script> 
import QuestionsContainer from './components/QuestionsContainer'
import Header from './components/layouts/Header'
import json from './assets/questions.json'

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
  background-color: lightgray;
}
.title{
  margin-top: 1.5em;
  margin-left: 2em;
  font-size: 2em;
}
</style>
