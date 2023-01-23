<template>
    <div class="learning">
      <h2>Learn the Words</h2>
      <!-- Display a message when there are no words in the list -->
      <div v-if="isWordsEmpty" class="empty-list-message">
        <p>There are no words in the list, please add a word you would like to learn.</p>
      </div>
      <!-- Display the learning content if there are words in the list -->
      <div v-else>
        <p class="learning-question">{{ currentWord.translation }}</p>
        <!-- toggle the visibility of the answer when the button is clicked -->
        <button @click="toggleAnswerVisibility" class="learning-button">{{ showAnswerButtonText }}</button>
        <p v-if="showAnswer" class="answer">{{ currentWord.word }}</p>
        <p class="learning-progress">{{ currentIndex + 1 }} / {{ words.length }}</p>
        <div class="learning-button-group">
          <!-- Display the next word button if the current index is less than the total number of words - 1 -->
          <button v-if="currentIndex < words.length - 1" class="next-word-button" @click="nextWord">Next Word</button>
          <button class="end-learning-button" @click="endLearning">End Learning</button>
        </div>
      </div>
    </div>
  </template>
  <script>
  export default {
    props: {
      words: {
        type: Array,
        required: true, // words prop is required for the component to function
      },
    },
    data() {
      return {
        currentIndex: 0, // the index of the current word being displayed
        showAnswer: false, // whether the answer is currently visible or not
      };
    },
    computed: {
      currentWord() {
        return this.shuffledWords[this.currentIndex]; // the current word object being displayed
      },
      showAnswerButtonText() {
        return this.showAnswer ? 'Hide Answer' : 'Show Answer'; // text for the show/hide answer button
      },
      isWordsEmpty() {
        return this.words.length === 0; // whether the words array is empty or not
      },
    },
    methods: {
      toggleAnswerVisibility() {
        this.showAnswer = !this.showAnswer; // toggle the visibility of the answer
      },
      nextWord() {
        this.showAnswer = false;
        this.currentIndex++; // move to the next word in the list
      },
      endLearning() {
        this.$emit('end'); // emit an "end" event to notify parent component that learning has ended
      },
      shuffleWords() {
        this.shuffledWords = this.words.sort(() => Math.random() - 0.5); // shuffle the words array
      },
    },
    created() {
      this.shuffleWords(); // shuffle the words when the component is created
    },
  };
  </script>