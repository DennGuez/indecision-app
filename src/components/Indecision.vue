<template>
    <img v-if="img" :src="img" alt="bg">
    <div class="bg-dark"></div>

    <div class="indecision-container">
        <input
          v-model="question"
          type="text"
          placeholder="Hazme una pregunta">
        <p>Recuerda terminar con nun signo de interrogacion (?)</p>

        <div v-if="isValidQuestion" >
            <h2>{{ question }}</h2>
            <h1>{{ answer === 'yes' ? 'Si!' : 'No!' }}</h1>
        </div>

    </div>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue'
const question = ref<string>('')
const answer = ref<string>('')
const img = ref<string>('')
const isValidQuestion = ref<boolean>(false)

const getAnswer = async () => {
    answer.value = 'Pensando...'
    const { answer: res, image } = await fetch('https://yesno.wtf/api').then( resp => resp.json() )
    // answer.value = res === 'yes' ? 'Si!' : 'No!'
    answer.value = res 
    img.value = image
}

watch(question, (value, oldValue) => { 
    isValidQuestion.value = false
    if ( !value.includes('?') ) return

    isValidQuestion.value = true
    console.log( isValidQuestion)
    
    getAnswer()

})


</script>

<style scoped>
img, .bg-dark {
    height: 100vh;
    left: 0px;
    max-height: 100%;
    max-width: 100%;
    position: fixed;
    top: 0px;
    width: 100vw;
}

.bg-dark {
    background-color: rgba(0, 0, 0, 0.4);
}

.indecision-container {
    position: relative;
    z-index: 99;
}

input {
    width: 250px;
    padding: 10px 15px;
    border-radius: 5px;
    border: none;
}
input:focus {
    outline: none;
}

p {
    color: white;
    font-size: 20px;
    margin-top: 0px;
}

h1, h2 {
    color: white;
}

h2 {
    margin-top: 150px;
}

</style>
