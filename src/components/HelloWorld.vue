<template>
  <div class="home">
   <div class="home__block">
     <h2>PASSAGE</h2>
     <textarea name="" id="" cols="30" rows="10" ref="passage" v-model="passage">
     </textarea>
   </div>
    
    <div class="home__block">
      <h2>QUESTION</h2>
      <input type="text" ref="question" @keyup.enter="answerQuestion" v-model="question">
    </div>

    <div class="home__block">
      <h2>ANSWERS</h2>
      <div class="home__block-answer" v-for="(answer, id) in answers" :key="answer">
        <span > {{ id + 1 }} : {{ answer.text }} ----- {{ answer.score }}</span>
      </div>
    </div>
    
  </div>
</template>

<script>
import * as tf from '@tensorflow/tfjs'
import * as qna from '@tensorflow-models/qna'
export default {
  name: "Home",
  data() {
    return {
      loadedModel: '',
      answers: [],
      question: '',
      passage: ''
    }
  },
  mounted() {
    this.loadModel()
  },
  methods: {
    async loadModel() {
      this.loadedModel = Object.freeze(await qna.load())
      const tensorflow = tf
      console.log('tensorflow : ', tensorflow);

    },
    async answerQuestion() {
      if(this.loadedModel !== null) {
        console.log('this.loadedModel : ', this.loadedModel);
        console.log('this.question : ', this.question);
        console.log('this.passage : ', this.passage);
        this.answers = await this.loadedModel.findAnswers(this.question, this.passage)
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.home {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  &__block {
    margin-top: 30px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  &__block-answer {
    margin-top: 8px;
    font-size: 18px;
  }
}

textarea {
  border-radius: 8px;
  width: 600px;
  height: 300px;
  outline: none;
  padding: 12px 16px;
  font-size: 16px;
}

input {
  border-radius: 4px;
  padding: 18px 16px;
  width: 600px;
  font-size: 16px;
  outline: none;
}

h2, span {
  color: whitesmoke;
}

span {
  margin-top: 8px;
}

</style>
