
//WordForm
<template>
  <div class="word-form">
    <h2>Add new word</h2>
    <!-- Form for adding new words -->
    <form @submit.prevent="addWord">
      <!-- Input for word -->
      <label>
        Word:
        <input type="text" v-model="newWord.word" required />
      </label>
      <br />
      <!-- Input for translation -->
      <label>
        Translation:
        <input type="text" v-model="newWord.translation" required />
      </label>
      <br />
      <!-- Add button, disabled when the form is empty -->
      <button type="submit" :disabled="newWord.word.trim() === '' || newWord.translation.trim() === ''">Add</button>
      <!-- Cancel button -->
      <button @click="cancelNewWordForm">Cancel</button>
    </form>
    <!-- Display a success message when a word is added -->
    <div v-if="message" class="success-message">{{ message }}</div>
  </div>
</template>
<script>
export default {
  name: 'WordForm',
  data() {
    return {
      // data for the new word
      newWord: {
        word: '',
        translation: ''
      },
      // success message
      message: ''
    }
  },
  methods: {
    // method to add a new word
    addWord() {
      // emit an add event to the parent component
      this.$emit('add', this.newWord);
      // clear the form
      this.clearForm();
      // set the success message
      this.message = 'Word is successfully added';
      // clear the success message after 3 seconds
      setTimeout(() => {
        this.message = '';
      }, 3000);
    },
    // method to cancel the form
    cancelNewWordForm() {
      // emit a cancel event to the parent component
      this.$emit('cancel');
    },
    // method to clear the form
    clearForm() {
      this.newWord = { word: '', translation: '' };
    }
  }
}
</script>