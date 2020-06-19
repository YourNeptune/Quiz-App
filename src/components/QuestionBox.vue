<template>
    <div class="questionbox-container">
    <b-jumbotron>
        
        <template v-slot:lead>
            {{currentQuestion.question}}
        </template>

        <hr class="my-4">

        <b-list-group>
            <b-list-group-item 
                v-for="(answer, index) in answers" 
                :key="index"
                @click.prevent="selectAnswer(index)"
                :class="checkAnswer(index)"
            >
                    {{answer}}
            </b-list-group-item>
        </b-list-group>


        <b-button 
            variant="primary"
            @click="submitAnswer"
            :disabled="selectedIndex === null || answered"
        >
            Submit
        </b-button>

        <b-button 
            class="nextButton"
            @click="next" 
            variant="success"
            :disabled="!answered || index === 9"
        >
            Next
        </b-button>

        <b-button 
            v-if="index === 9 && answered"
            variant="warning"
            @click="resetQuestions"
        >
            Reset
        </b-button>

    </b-jumbotron>
</div>
</template>

<script>
import lodash from 'lodash'

export default {
    props:{
        currentQuestion: Object,
        next:Function,
        increment:Function,
        index: Number
    },
    data(){
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers:[],
            answered: false,
            
        }
    },
    computed:{
        answers(){
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    },
    methods:{
        selectAnswer(index){
            this.selectedIndex = index
        },
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = lodash.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswer(){
            let isCorrect = false
            if(this.selectedIndex === this.correctIndex) {
                isCorrect = true
            }
            this.answered = true
            this.increment(isCorrect)
        },
        checkAnswer(index){
            let addClass = ''
            if(!this.answered && this.selectedIndex === index){
                addClass = 'selected'
            }else if(this.answered && this.correctIndex === index){
                addClass = 'correct'
            }else if(this.answered && this.selectedIndex === index && this.correctIndex !== index){
                addClass = 'incorrect'
            }
            return addClass
        },
        resetQuestions(){
            location.reload()
        }
    },
    watch: {
        currentQuestion: {
            // shuffle the questions at the first loading time and each updating time
            immediate:true,     
            handler() {
               
                this.answered = false
                this.selectedIndex = null
                this.shuffleAnswers()
            }
            
        }
    }
}
</script>

<style scoped>
.questionbox-container {
    margin-top: 40px;
}
/* find in inspect */
.list-group {   
    margin-bottom: 20px;
}
.list-group-item:hover {
    background: #E9ECEF;
    cursor: pointer;
}
.btn {
    margin: 0 10px;
}

.selected {
    background: #a6dcef;
}
.correct {
    background: #21bf73;
}
.incorrect {
    background: #fd5e53;
}

</style>