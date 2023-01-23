<template>
  <div class="app">
    <!-- navigation bar to switch between different views -->
    <nav>
      <button @click="currentView = 'list'">Word List</button> <!-- button to switch view to word list -->
      <button @click="currentView = 'add'">Add Word</button> <!-- button to switch view to add word form -->
      <button @click="currentView = 'quiz'">Quiz</button> <!-- button to switch view to quiz -->
      <button @click="currentView = 'matchingQuiz'">Matching Quiz</button> <!-- button to switch view to matching quiz -->
      <button @click="currentView = 'learning'">Learn</button> <!-- button to switch view to learning -->
    </nav>
    <!-- conditionally render different child components based on currentView -->
    <word-list v-if="currentView === 'list'" :words="words" @remove="removeWord" @edit="editWord" @add="addWord" />
    <!-- render word-list component and pass in words data and listen for remove, edit and add events -->
    <word-form v-else-if="currentView === 'add'" @add="addWord" @cancel="cancelNewWordForm" />
    <!-- render word-form component and listen for add and cancel events -->
    <quiz v-else-if="currentView === 'quiz'" :words="words" @end="endQuiz" />
    <!-- render quiz component and pass in words data and listen for end event -->
    <matching-quiz v-else-if="currentView === 'matchingQuiz'" :words="words" @end="endQuiz" />
    <!-- render matching-quiz component and pass in words data and listen for end event -->
    <learning v-else-if="currentView === 'learning'" :words="words" @end="endLearning" />
    <!-- render learning component and pass in words data and listen for end event -->
  </div>
</template>
<script>
// import the child components
import WordList from './components/WordList.vue';
import WordForm from './components/WordForm.vue';
import Quiz from './components/Quiz.vue';
import MatchingQuiz from './components/MatchingQuiz.vue';
import Learning from './components/Learning.vue';
export default {
  name: 'App',
  // register the child components
  components: {
    WordList,
    WordForm,
    Quiz,
    MatchingQuiz,
    Learning
  },
  data() {
    return {
      // list of words
      words: [],
      // current view
      currentView: 'list'
    }
  },
  methods: {
    // method to add word
    addWord(word) {
      this.words.push(word);
    },
    // method to remove word
    removeWord(index) {
      this.words.splice(index, 1);
    },
    // method to edit word
    editWord(editInfo) {
      this.words.splice(editInfo.index, 1, editInfo);
    },
    // method to cancel the new word form 
    cancelNewWordForm() {
      this.currentView = 'list';
    },
    // method to handle the end of quiz
    endQuiz() {
      this.currentView = 'list';
    },
    // method to handle the end of learning
    endLearning() {
      this.currentView = 'list';
    }
  }
}
</script>