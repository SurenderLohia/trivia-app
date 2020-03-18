<template>
  <div class="flex-item">
    <div v-bind:class="['has-text-centered', isLoading ? '' : 'hide']">Loading...</div>
    <div v-bind:class="[isLoading ? 'hide' : '']">
      <h3 class="is-size-6 has-text-centered">Round {{currentTriviaNumber}}/{{triviaList.length}}</h3>
      <h1 class="is-size-1 has-text-centered mb1">0:07</h1>
      <div class="trivia-main-section mb2">
        <h2 class="is-size-3 question has-text-centered mb2">In which city, is the Big Nickel located in Canada?</h2>
        <div class="columns is-multiline is-variable is-7">
          <div class="column is-half">
            <div class="answer-item">
              <span>1. Victoria, British Columbia</span>
            </div>
          </div>
          <div class="column is-half">
            <div class="answer-item">
              <span>1. Victoria, British Columbia</span>
            </div>
          </div>
          <div class="column is-half">
            <div class="answer-item">
              <span>1. Victoria, British Columbia</span>
            </div>
          </div>
          <div class="column is-half">
            <div class="answer-item">
              <span>1. Victoria, British Columbia</span>
            </div>
          </div>
        </div>
      </div>
      <div>
        <h6 class="is-size-5 has-text-centered">Select correct answer. One entry per round</h6>
      </div>
    </div>
  </div>
</template>


<script>
import axios from 'axios';
export default {
  name: 'Triva',
  data() {
    return {
      triviaList: [],
      isLoading: true,
      currentTriviaNumber: 1,
    }
  },
  created () {
    const url = 'https://opentdb.com/api.php?amount=20&category=9&difficulty=easy&type=multiple';
    axios.get(url)
      .then((response) => {
        this.triviaList = response.data.results;
        this.isLoading = false;
      })
      .catch((error) => {
        console.log(error);
      });
  }
}
</script>
