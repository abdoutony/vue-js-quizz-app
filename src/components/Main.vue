<template>
  <div class="container">
    <div class="col-md-12">
        <div class="row">
          <div class="d--f jc--c">
            <ShowScore :score="this.quizScore" v-if="showScore" />
            <div class="card col-md-9"
             v-if="!showScore" style="padding:10px">
                <div class="card-body">
                    <div class="row d--f jc--c">
                       <div class="text-center">
                          <h1 class="text-center">Take your quiz now</h1>
                          <p>You Have 10 Minutes To Finish The Quiz</p>
                       </div>
                    </div>

                    <div class="row question-r-t">
                       <QuestionsRemaining :questionsremaining="calculateQuestionsRemaining" />
                        <Timer :timer="this.timer" />                      
                    </div>

                    <div> 
                       <QuizInfo :category="this.category" :difficulty="this.difficulty" 
                       :question="this.question"/>
                       <form>
                            <div class="d--f jc--c">
                              <div class="inputcontainer">
                               <input type="radio"
                                name="answer" 
                                value="True"
                                 v-model="form.answer"
                                 :disabled="quizFinished" /> True
                              </div> 
                            <div class="inputcontainer">
                                <input type="radio"
                                 name="answer"
                                 value="False"
                                 v-model="form.answer" 
                                :disabled="quizFinished"/> False
                            </div>
                            </div>
                            <div class="row" style="padding-top:30px;padding-bottom:10px">
                                <div class="col-md-6 text-center">
                                     <button class="btn btn-danger"
                                     @click.prevent="perviousQuestion"
                                     :disabled="isPerviousBtnDisabled">Pervious Question</button>
                                </div>

                                <div class="col-md-6 text-center">
                                   <button type="submit" class="btn btn-primary"
                                    @click.prevent="setAnswer"
                                    v-if="!quizFinished">Next Question</button>
                                    <button class="btn btn-success"    
                                    v-if="quizFinished"
                                    @click.prevent="calculateQuizScore">
                                        Finish and See Results
                                    </button>
                                </div>

                            </div>
                        </form>

                    </div>

                   
            </div>      
          </div>
        </div>
    </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Vue from 'vue'
import Form from 'vform'
import ShowScore from './include/ShowScore.vue';
import QuestionsRemaining from './include/QuestionsRemaining.vue';
import Timer from './include/Timer.vue';
import QuizInfo from './include/QuizInfo.vue';
export default {
  name:"Maincomponent",
  components: { ShowScore, QuestionsRemaining,Timer, QuizInfo },

  data(){
      return{
        apiUrls:[
            'https://opentdb.com/api.php?amount=10&category=9&difficulty=easy&type=boolean',
            'https://opentdb.com/api.php?amount=10&category=11&difficulty=easy&type=boolean',
            'https://opentdb.com/api.php?amount=10&category=15&difficulty=easy&type=boolean',
            'https://opentdb.com/api.php?amount=10&category=17&difficulty=easy&type=boolean'
        ],
        form : new Form({
          answer:'',
        }),
        quiz:[],
        category:'',
        difficulty:'',
        question:'',
        answers:[],
        questionindex:0,
        quizFinished:false,
        quizScore:0,
        showScore:false, 
        isPerviousBtnDisabled:false,
        timer:600
      }
  },
  methods:{
     getQuiz(){
         axios.get(this.apiUrls[1])
         .then((response)=>{
             this.quiz = response.data['results']
             console.log(this.quiz)
             this.category = this.quiz[this.questionindex].category
             this.difficulty = this.quiz[this.questionindex].difficulty
             this.question = this.quiz[this.questionindex].question
         })
     },
     setQuizFinished(){
        this.quizFinished = true
     },
     setShowScore(){
        this.showScore = true
     },
     setAnswer(){
        Vue.set(this.answers,this.answers.length,this.form.answer)
        if(this.questionindex == (this.quiz.length -1)){
            this.setQuizFinished()
        }else{
            this.questionindex += 1
            this.category = this.quiz[this.questionindex].category
            this.difficulty = this.quiz[this.questionindex].difficulty
            this.question = this.quiz[this.questionindex].question
            this.form.reset()
        }

        if(this.questionindex > 0){
            this.enablePerviousBtn()
        }

        
     },
     disablePerviousBtn(){
        this.isPerviousBtnDisabled = true
     },
     enablePerviousBtn(){
       this.isPerviousBtnDisabled = false
     },
     perviousQuestion(){
           if(this.questionindex >0){
                this.questionindex -= 1
                this.category = this.quiz[this.questionindex].category
                this.difficulty = this.quiz[this.questionindex].difficulty
                this.question = this.quiz[this.questionindex].question
                this.answers.pop()
           }
           if(this.questionindex == 0){
               this.disablePerviousBtn()
           }
           

     },
     calculateQuizScore(){

         for(let i = 0 ; i< this.answers.length; i++){
             if (this.answers[i] == this.quiz[i].correct_answer){
                  this.quizScore +=10;
             }
         }
         this.setShowScore()
         console.log(this.quizScore)
     },
     stint(){
      setInterval(function(){ 
          this.timer = this.timer-1
          if(this.timer == 0 ){
             this.calculateQuizScore()
          }
          }.bind(this), 1000);
     }
  },

  mounted(){
    this.getQuiz()
    this.disablePerviousBtn()
    this.stint()
  },
  computed:{
      calculateQuestionsRemaining(){
          return this.quiz.length - this.questionindex
      }
  }

}
</script>

<style>

</style>