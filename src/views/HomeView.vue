<template>
  <div>
    <BookList
      v-if="!isFormVisible"
      :books="books"
      @add="showForm"
      @edit="editBook"
      @delete="deleteBook"
    />
    <BookForm
      v-else
      :book="selectedBook"
      @save="saveBook"
      @cancel="hideForm"
    />
  </div>
</template>

<script>
import BookList from "../components/BookList.vue";
import BookForm from "../components/BookForm.vue";

export default {
  components: { BookList, BookForm },
  data() {
    return {
      books: [],
      isFormVisible: false,
      selectedBook: null,
    };
  },
  methods: {
    showForm() {
      this.selectedBook = { id: null, title: "", author: "" };
      this.isFormVisible = true;
    },
    editBook(book) {
      this.selectedBook = { ...book };
      this.isFormVisible = true;
    },
    deleteBook(id) {
      this.books = this.books.filter((book) => book.id !== id);
    },
    saveBook(book) {
      if (book.id) {
        const index = this.books.findIndex((b) => b.id === book.id);
        if (index !== -1) this.books[index] = book;
      } else {
        book.id = Date.now();
        this.books.push(book);
      }
      this.hideForm();
    },
    hideForm() {
      this.isFormVisible = false;
    },
  },
};
</script>
