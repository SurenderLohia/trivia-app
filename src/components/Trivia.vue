<template>
  <div class="flex-item">
    <div class="has-text-centered" v-if="currentView === 'loader'">Loading...</div>
    <div v-if="currentView === 'quiz'">
      <h3 class="is-size-6 has-text-centered">Round {{currentTriviaIndex + 1}}/{{triviaList.length}}</h3>
      <h1 class="is-size-1 has-text-centered mb1">{{getHours(totalSeconds)}}:{{getSeconds(totalSeconds)}}</h1>
      <div class="trivia-main-section mb2">
        <h2 class="is-size-3 question has-text-centered wrapword mb2">{{ triviaList.length && triviaList[currentTriviaIndex].question }}</h2>
        <div class="columns is-multiline is-variable is-7">
          <div class="column is-half" v-for="option in triviaList[currentTriviaIndex].options" :key="option.question">
            <button v-bind:class="['answer-item', option === selectedOption ? 'active' : '' ]" v-on:click="onSelectOption(option, currentTriviaIndex)">{{option}}</button>
          </div>
        </div>
      </div>
      <div>
        <div class="columns">
          <div class="column has-text-right">
            <button class="button is-secondory" :disabled="currentTriviaIndex === 0" v-on:click="gotoPrevious">Previous</button>
            <button class="button is-primary ml1" :disabled="(currentTriviaIndex + 1) === triviaList.length" v-on:click="gotoNext">Next</button>
            <button class="button is-primary ml1" v-if="(currentTriviaIndex + 1) === triviaList.length" v-on:click="onSubmit">Submit</button>
          </div>
        </div>
        <h6 class="is-size-5 has-text-centered">Select correct answer. One entry per round</h6>
      </div>
    </div>
    <div class="result" v-if="currentView === 'result'">
      <h2 class="is-size-3 has-text-centered">Congratulation! You successfully completed this trivia </h2>
      <h1 class="is-size-1 has-text-centered">Total score: {{totalScore}} </h1>
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
      currentTriviaIndex: 0,
      currentTrivia: {},
      selectedOptions: {},
      totalScore: 0,
      currentView: 'loader', // Possible views or loader, quiz, result,
      selectedOption: '',
      countFrom: new Date(),
      totalSeconds: 0
    }
  },
  created () {
    const url = 'https://opentdb.com/api.php?amount=20&category=9&difficulty=easy&type=multiple';
    axios.get(url)
      .then((response) => {
        this.triviaList = response.data.results.slice(0, 2);
        this.isLoading = false;
        this.currentTrivia = this.triviaList[this.currentTriviaIndex]
        this.currentView = 'quiz'

        this.addTriviaOptions();
        this.start()
      })
      .catch((error) => {
        console.log(error);
      });
  },
  methods: {
    getTriviaOptions(trivaItem) {
      console.log('come here');
      const triviaOptions = trivaItem && [trivaItem.correct_answer, ...trivaItem.incorrect_answers];

      function shuffle(array = []) {
        for (let i = array.length - 1; i > 0; i--) {
          let j = Math.floor(Math.random() * (i + 1));
          [array[i], array[j]] = [array[j], array[i]];
        }

        return array;
      }
      
      return shuffle(triviaOptions);
    },
    
    addTriviaOptions() {
      this.triviaList = this.triviaList.map((triviaItem) => {
        const options = this.getTriviaOptions(triviaItem);

        return {...triviaItem, options}
      });
    },

    onSelectOption(option, currentTriviaIndex) {
      this.selectedOptions[currentTriviaIndex] = option;
      console.log('selectedOptions', this.selectedOptions[currentTriviaIndex]);
      this.selectedOption = option;
      console.log('cool');
    },

    gotoNext() {
      this.currentTriviaIndex++;
    },

    gotoPrevious() {
      this.currentTriviaIndex--;
    },

    getTotalScore() {
      let total = 0;
      const QUESTION_WEIGHT = 5;

      for (const key in this.selectedOptions) {
        if (Object.prototype.hasOwnProperty.call(this.selectedOptions, key)) {
          const selectedOption = this.selectedOptions[key];
          if(selectedOption === this.triviaList[key].correct_answer) {
            total += QUESTION_WEIGHT;
          }
        }
      }

      this.totalScore = total;
    },

    onSubmit() {
      this.getTotalScore();
      this.currentView = 'result';
    },

    start() {
      this.isRunning = true
        if (!this.timer) {
          this.timer = setInterval( () => {
            this.totalSeconds++
          }, 1000 )
      }
    },

    getSeconds (totalSeconds) {
      let result = totalSeconds % 60;
      return String(result).length === 1 ? `0${result}` : result;
    },

    getHours(totalSeconds) {
      return Math.floor(totalSeconds / (60 * 60));
    }

  }
}
</script>
