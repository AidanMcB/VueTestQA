<template>
<div>
    <div v-bind:key="question.id" v-for="question in questions">
        <QuestionCard v-bind:question="question" />
    </div>
</div>
</template>

<script>
import QuestionCard from './QuestionCard'
import json from '../assets/questions.json'
console.log(json)
export default {
    name: "QuestionsContainer",
    components:  {
        QuestionCard
    },
    props: ["questions"]
}
</script>

<style scoped>

</style>