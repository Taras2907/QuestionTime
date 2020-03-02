
<template>
  <div class="home">
    <div class="container mt-2">
      <div v-for="question in questions"
          :key="question.pk"
      >
        <p class="mb-0">Posted by:
          <span class="question-auhtor-name">{{question.author}}</span>
        </p>
        <router-link
                :to="{ name: 'question', params: {slug: question.slug }}"
                class="question-link"
        >{{question.content}}
        </router-link>
        <p>Answers: {{question.answers_count}}</p>
      </div>
      <div class="my-4">
        <p v-show="loadingQuestions">....loading...</p>
        <button
                class="btn btn-sm btn-success btn-outline"
                v-show="next"
                @click="getQuestions"
        >Load More</button>
      </div>
    </div>
  </div>
</template>

<script>
   /* eslint-disable */
import { apiService } from "@/common/api.service";

export default {
  name: "home",
  data(){
    return{
      questions:[],
      next:null,
      loadingQuestions:false
    }
  },
  methods:{
    getQuestions(){
      let endpoint = 'api/questions/';
      if (this.next){
        endpoint = this.next;
      }
      this.loadingQuestions = true;
      apiService(endpoint)
              .then(data => {
                this.questions.push(...data.results);
                this.loadingQuestions = false;
                if (data.next){
                  this.next = data.next
                }else {
                  this.next = null
                }
              });
    }
  },
  created() {
    this.getQuestions();
  }
};
</script>
<style scoped>
  .question-auhtor-name{
    font-weight: bold;
    color: red;
  }
  .question-link{
    font-weight: bold;
    font-size: 18px;
    color: black;

  }
  .question-link:hover{
    font-weight: bold;
    color: lightgray;
    text-decoration: none;

  }
</style>
