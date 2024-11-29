<template>
  <div class="min-h-screen bg-gray-100 p-8">
    <div class="container mx-auto">
      <h1 class="text-2xl font-bold mb-6">Manajemen Buku</h1>

      <!-- Menampilkan form untuk menambah dan mengedit buku -->
      <BookForm
        :form="form"
        :editIndex="editIndex"
        :saveBook="saveBook"
      />

      <!-- Menampilkan daftar buku -->
      <BookList
        :books="books"
        :editBook="editBook"
        :deleteBook="deleteBook"
        :onClickBook="openBookDetail"
      />

      <!-- Modal Detail Buku untuk Peminjaman dan Pengembalian -->
      <BookBorrow
        :showModal="showDetailModal"
        :book="currentBook"
        :closeModal="closeBookDetail"
        :updateBookList="updateBookList"
      />
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import BookList from "./components/BookList.vue";
import BookForm from "./components/BookForm.vue";
import BookBorrow from "./components/BookBorrow.vue";

export default {
  components: { BookList, BookForm, BookBorrow },
  setup() {
    const books = ref([
      { judul: "Vue.js untuk Pemula", pengarang: "John Doe", tahun: "2022", jumlah: 5, pinjam: 0 },
      { judul: "Mastering JavaScript", pengarang: "Jane Smith", tahun: "2020", jumlah: 3, pinjam: 0 },
    ]);

    const form = ref({
      judul: "",
      pengarang: "",
      tahun: "",
      jumlah: "",
    });

    const editIndex = ref(null);
    const currentBook = ref(null);
    const showDetailModal = ref(false);

    // Fungsi untuk menyimpan buku baru atau perubahan buku
    const saveBook = () => {
      if (editIndex.value !== null) {
        books.value[editIndex.value] = { ...form.value };
        editIndex.value = null;
      } else {
        books.value.push({ ...form.value });
      }
      resetForm();
    };

    // Fungsi untuk mengedit buku
    const editBook = (index) => {
      editIndex.value = index;
      form.value = { ...books.value[index] };
    };

    // Fungsi untuk menghapus buku
    const deleteBook = (index) => {
      books.value.splice(index, 1);
    };

    // Fungsi untuk membuka detail buku
    const openBookDetail = (book) => {
      currentBook.value = book;
      showDetailModal.value = true;
    };

    // Fungsi untuk menutup modal detail buku
    const closeBookDetail = () => {
      showDetailModal.value = false;
    };

    // Fungsi untuk memperbarui daftar buku
    const updateBookList = (updatedBook) => {
      const index = books.value.findIndex((book) => book.judul === updatedBook.judul);
      if (index !== -1) {
        books.value[index] = { ...updatedBook };
      }
    };

    // Fungsi untuk mengatur ulang formulir setelah menyimpan buku
    const resetForm = () => {
      form.value = {
        judul: "",
        pengarang: "",
        tahun: "",
        jumlah: "",
      };
    };

    return {
      books,
      form,
      editIndex,
      saveBook,
      editBook,
      deleteBook,
      openBookDetail,
      closeBookDetail,
      showDetailModal,
      currentBook,
      updateBookList,
    };
  },
};
</script>
