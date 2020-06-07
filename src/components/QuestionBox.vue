<template>
    <div class="question-box-container">       
        <b-jumbotron>
            <template v-slot:lead>
                {{currentQuestion.question}}
            </template>

            <hr class="my-4">


            <b-list-group >
                <b-list-group-item
                    v-for="(answer, index) in shuffledAnswers" :key="index"
                    @click.prevent="selectedAnswer(index)"
                    :class="[
                        !isAnswered && selectedIndex === index? 'selected-answer' : 
                        isAnswered && correctIndex === index ? 'correct-answer' :
                        isAnswered && selectedIndex === index && correctIndex != index ? 'incorrect-answer': ''
                        ]"
                >
                    {{answer}}
                </b-list-group-item>
            </b-list-group>


            <b-button 
                variant="primary"
                @click="submitAnswer"
                :disabled="selectedIndex===null || isAnswered"
            >

                Submit
            </b-button>
            <b-button @click="next" variant="success" href="#">Next</b-button>
        </b-jumbotron>
    </div>
  </template>

<script>
    import _ from 'lodash'
  export default {
    props: {
      currentQuestion: Object,
      next: Function,
      increment: Function
    },
    data(){
        return{
            selectedIndex:null, 
            correctIndex:null,
            shuffledAnswers:[],
            isAnswered: false
        }
    },
    computed:{
        //this one is done once in shiffleAnswers
        //answers(){
          //  const answers=[...this.currentQuestion.incorrect_answers]
            //answers.push(this.currentQuestion.correct_answer)
            //return answers
        //}
    },
    watch:{
        currentQuestion:{
            immediate: true,
            handler(){
                this.selectedIndex=null
                this.isAnswered=null
                this.shuffleAnswers()
            }
        }
    },
    methods:{
        selectedAnswer(index){
            this.selectedIndex= index
        },
        shuffleAnswers(){
            const answers=[...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers= _.shuffle(answers)
            this.correctIndex=this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswer(){
            let isCorrect=false

            if(this.selectedIndex === this.correctIndex){
                isCorrect=true  
            }
            this.isAnswered=true
            this.increment(isCorrect)
        }
    }
  }
</script>
<style scoped>
    .list-group{
        margin-bottom: 15px;
    }

    .list-group-item:hover{
        background: #EEE;
        cursor: pointer;
    }

    .btn{
        margin: 0 5px;
    }

    .selected-answer{
        background-color: lightblue;
    }
    .correct-answer{
        background-color: lightgreen;
    }
    .incorrect-answer{
        background-color: lightgoldenrodyellow;
    }

</style>