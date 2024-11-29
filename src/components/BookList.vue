<template>
  <div class="bg-white p-6 rounded-lg shadow-md">
    <h2 class="text-xl font-semibold mb-4">Daftar Buku</h2>
    <div v-if="books.length === 0" class="text-center mt-4 text-gray-500">
      Tidak ada buku.
    </div>
    <div v-else class="flex flex-wrap gap-4">
      <div
        v-for="(buku, index) in books"
        :key="index"
        class="flex items-start space-x-4 p-4 bg-gray-100 rounded-lg shadow-sm w-full md:w-1/3 lg:w-1/4"
      >
        <div class="flex-1 text-center">
          <!-- Menambahkan event klik hanya pada judul buku -->
          <div
            class="font-semibold text-xl mb-2 cursor-pointer"
            @click="openBookDetail(buku)"
          >
            {{ buku.judul }}
          </div>
          <div class="text-sm text-gray-600 mb-1">Pengarang: {{ buku.pengarang }}</div>
          <div class="text-sm text-gray-600 mb-1">Tahun Terbit: {{ buku.tahun }}</div>
          <div class="text-sm text-gray-600 mb-2">Jumlah Buku: {{ buku.jumlah }}</div>
        </div>
        <div class="flex flex-col justify-between space-y-2">
          <!-- Tombol Edit, hanya menangani klik untuk mengedit -->
          <button
            @click.stop="editBook(index)"
            class="text-blue-500 hover:text-blue-700"
          >
            <i class="fas fa-edit"></i>
          </button>
          <!-- Tombol Hapus, hanya menangani klik untuk menghapus -->
          <button
            @click.stop="deleteBook(index)"
            class="text-red-500 hover:text-red-700"
          >
            <i class="fas fa-trash-alt"></i>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    books: Array,
    editBook: Function, // Fungsi untuk mengedit buku
    deleteBook: Function, // Fungsi untuk menghapus buku
    onClickBook: Function, // Fungsi untuk membuka detail buku saat judul diklik
  },
  methods: {
    // Fungsi untuk membuka detail buku
    openBookDetail(book) {
      this.onClickBook(book); // Memanggil fungsi di parent (App.vue) untuk menampilkan detail buku
    }
  }
};
</script>

<style scoped>
/* Menambahkan ikon fas (Font Awesome) untuk edit dan delete button */
@import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css');
</style>
