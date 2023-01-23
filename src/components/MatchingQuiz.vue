// MatchingQuiz.vue
<template>
  <div class="matching-quiz">
    <h2>Matching Quiz</h2>
    <!-- Display a message if there are no words in the list -->
    <div v-if="isWordsEmpty" class="empty-list-message">
      <p>There are no words in the list, please add a word you would like to learn.</p>
    </div>
    <div v-else>
      <!-- Loop through the shuffled words and display the translation, select input and feedback -->
      <div v-for="(word, index) in shuffledWords" :key="index" class="matching-quiz-word">
        <p>{{ word.translation }}</p>
        <select v-model="selectedAnswers[index]" class="matching-quiz-select">
          <option v-for="word in words" :key="word.word">{{ word.word }}</option>
        </select>
        <!-- Display feedback if the answer is correct or incorrect -->
        <p v-if="feedback[index] === 'Correct'" class="matching-quiz-feedback correct">Correct</p>
        <p v-else-if="feedback[index] === 'Incorrect'" class="matching-quiz-feedback incorrect">Incorrect ({{
          word.word
        }})</p>
      </div>
      <!-- Display submit and end quiz buttons if the quiz has not been submitted yet -->
      <button v-if="!submitted" @click="checkAnswers" class="end-quiz-button">Submit</button>
      <button v-if="!submitted" @click="endQuiz" class="end-quiz-button">End Quiz</button>
      <!-- Display play again and end quiz buttons if the quiz has been submitted and results are shown -->
      <div v-if="showResults" class="matching-quiz-result-buttons">
        <button @click="playAgain">Play Again</button>
        <button @click="endQuiz">End Quiz</button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ['words'],
  data() {
    return {
      selectedAnswers: [], // array to hold the selected answers
      feedback: [], // array to hold feedback for each selected answer
      showResults: false, // boolean value to determine if the results should be displayed
      submitted: false, // boolean value to determine if the form has been submitted
    };
  },
  computed: {
    isWordsEmpty() {
      return this.words.length === 0; // check if the words prop is empty
    },
    shuffledWords() {
      return this.words.sort(() => Math.random() - 0.5); // shuffle the words array
    }
  },
  methods: {
    checkAnswers() {
      this.feedback = [];
      this.words.forEach((word, index) => { // loop through the words array
        if (word.word === this.selectedAnswers[index]) { // check if the selected answer matches the word
          this.feedback.push('Correct');
        } else {
          this.feedback.push('Incorrect');
        }
      });
      this.submitted = true; // set submitted to true
      this.showResults = true; // set showResults to true to display the results
    },
    playAgain() {
      this.selectedAnswers = []; // reset the selected answers array
      this.feedback = []; // reset the feedback array
      this.showResults = false; // hide the results
      this.submitted = false; // set submitted to false
    },
    endQuiz() {
      this.$emit('end') // emit an event to the parent component to handle ending the quiz
    }
  },
};
</script>