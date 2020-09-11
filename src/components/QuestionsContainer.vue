<template>
<div>
    <div class="results">
<!-- conditionally render quiz results -->
    </div>
    <form @submit="handleSubmit">
    <div v-bind:key="question.id" v-for="question in questions">
        <QuestionCard v-bind:question="question" v-on:addAnswer="handleChoose($event)"/>
    </div>
    <input type="submit" value="Submit" class="btn">
    </form>
    <button @click="printThis('x')">here</button>
</div>
</template>

<script>
import QuestionCard from './QuestionCard'
// import json from '../assets/questions.json'
export default {
    name: "QuestionsContainer",
    components:  {
        QuestionCard
    },
    data() {
        return {
            test: []
        }
    },
    props: ["questions"],
    methods: {
        handleSubmit(e) {
            e.preventDefault()
            console.log("quiz submitted")
        },
        printThis(x){
            console.log(x)
        },
        handleChoose(choice){
            this.test = [...this.test, choice]
            console.log(this.test)
        }
    }
}
</script>

<style scoped>

</style>