
//WordList
<template>
  <div class="word-list">
    <h2>Word List</h2>
    <!-- Display a message when there are no words in the list -->
    <div v-if="words.length === 0" class="empty-list-message">
      <p>There are no words in the list, please add a word you would like to learn.</p>
    </div>
    <!-- Display the table of words when there are words in the list -->
    <table v-else>
      <thead>
        <tr>
          <th>Word</th>
          <th>Translation</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(word, index) in paginatedWords" :key="index">
          <td>{{ word.word }}</td>
          <td>{{ word.translation }}</td>
          <td>
            <button @click="editWord(index)">Edit</button>
            <button @click="removeWord(index)">Remove</button>
          </td>
        </tr>
      </tbody>
    </table>
    <!-- Pagination buttons and current page/total pages display -->
    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
      <span>{{ currentPage }} / {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
    </div>
    <!-- Form for editing and adding words -->
    <form v-if="editing" @submit.prevent="saveWord">
      <label for="word">Word:</label>
      <input type="text" id="word" v-model="word" />
      <label for="translation">Translation:</label>
      <input type="text" id="translation" v-model="translation" />
      <button type="submit">Save</button>
      <button @click="cancelEdit">Cancel</button>
    </form>
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
      editing: false,
      editedIndex: -1,
      word: '',
      translation: '',
      wordsPerPage: 10,
      currentPage: 1,
    };
  },
  computed: {
    totalPages() {
      if (this.words.length === 0) {
        return 1;
      }
      // compute the total number of pages based on the wordsPerPage and the number of words
      return Math.ceil(this.words.length / this.wordsPerPage);
    },
    paginatedWords() {
      // slice the words array to get the words for the current page
      const start = (this.currentPage - 1) * this.wordsPerPage;
      return this.words.slice(start, start + this.wordsPerPage);
    },
  },
  methods: {
    // method to go to the next page of words
    nextPage() {
      if (this.currentPage <= this.totalPages) {
        this.currentPage++;
      }
    },
    // method to go to the previous page of words
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    // method to save a word
    saveWord() {
      if (this.editedIndex === -1) {
        this.$emit('add', { word: this.word, translation: this.translation });
      } else {
        this.$emit('edit', {
          index: this.editedIndex,
          word: this.word,
          translation: this.translation,
        });
      }
      // reset editing state
      this.cancelEdit();
    },
    // method to remove a word
    removeWord(index) {
      this.$emit('remove', index);
    },
    // method to edit a word
    editWord(index) {
      this.editing = true;
      this.editedIndex = index;
      this.word = this.words[index].word;
      this.translation = this.words[index].translation;
    },
    // method to reset the editing state and clear the form inputs
    cancelEdit() {
      this.editing = false;
      this.editedIndex = -1;
      this.word = '';
      this.translation = '';
    },
  },
};
</script>