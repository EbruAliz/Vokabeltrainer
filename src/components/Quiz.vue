//Quiz.vue
<template>
  <div class="quiz">
    <h2>Quiz</h2>
    <!-- Display a message when there are no words in the list -->
    <div v-if="isWordsEmpty" class="empty-list-message">
      <p>There are no words in the list, please add a word you would like to learn.</p>
    </div>
    <div v-else>
      <!-- Display the translation of the current word as a question -->
      <p class="quiz-question">{{ word.translation }}</p>
      <!-- Form for answering the quiz question -->
      <form v-if="!submitted" @submit.prevent="checkAnswer">
        <label for="quiz-answer" class="quiz-label">Enter word:</label>
        <input id="quiz-answer" v-model="answer" class="quiz-input" />
        <br />
        <button type="submit" class="quiz-button">Submit</button>
        <button class="quiz-button" @click="endQuiz">End Quiz</button>
      </form>
      <!-- Feedback for quiz answer -->
      <p v-if="submitted" :class="{ 'quiz-feedback': true, 'incorrect': feedback === 'Incorrect' }">{{ feedback }}</p>
      <!-- Display the correct word when answer is incorrect -->
      <p v-if="submitted && feedback === 'Incorrect'" class="correct-word">Correct word: {{ word.word }}</p>
      <!-- Display the current progress of the quiz -->
      <p class="quiz-progress">{{ currentIndex + 1 }} / {{ words.length }}</p>
      <!-- Button for going to the next word when the answer is correct and there are more words to quiz -->
      <button v-if="submitted && feedback === 'Correct' && currentIndex < words.length - 1" class="next-word-button"
        @click="nextWord">Next Word</button>
      <!-- Button for trying again when the answer is incorrect -->
      <button v-if="submitted && feedback === 'Incorrect'" class="try-again-button" @click="tryAgain">Try Again</button>
      <!-- Button for ending the quiz -->
      <button v-if="submitted" class="end-quiz-button" @click="endQuiz">End Quiz</button>
    </div>
  </div>
</template>
   
<script>
export default {
  props: {
    words: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      currentIndex: 0,
      answer: '',
      feedback: '',
      submitted: false,
      shuffledWords: []
    };
  },
  computed: {
    word() {
      return this.shuffledWords[this.currentIndex]
    },
    isWordsEmpty() {
      return this.words.length === 0;
    }
  },
  created() {
    this.shuffleWords()
  },
  methods: {
    // shuffle the order of words in the list
    shuffleWords() {
      this.shuffledWords = this.words.sort(() => Math.random() - 0.5)
    },
    // check if the answer is correct
    checkAnswer() {
      this.submitted = true;
      if (this.answer.toLowerCase() === this.word.word.toLowerCase()) {
        this.feedback = 'Correct';
      } else {
        this.feedback = 'Incorrect';
      }
    },
    // method to try the current word again
    tryAgain() {
      this.answer = '';
      this.feedback = '';
      this.submitted = false;
    },
    // method to go to the next word
    nextWord() {
      this.currentIndex++;
      if (this.currentIndex >= this.shuffledWords.length) {
        this.endQuiz();
      } else {
        this.answer = '';
        this.feedback = '';
        this.submitted = false;
      }
    },
    // method to end the quiz
    endQuiz() {
      this.$emit('end');
    },
  },
};
</script>