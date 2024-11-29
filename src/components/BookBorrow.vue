<template>
    <div
      v-if="showModal"
      class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 z-50"
    >
      <!-- Modal container -->
      <div class="bg-white p-6 rounded-lg w-96">
        <!-- Close button (icon X) -->
        <div class="flex justify-end">
          <button @click="closeModal" class="text-xl font-bold text-gray-500 hover:text-gray-700">
            &times;
          </button>
        </div>
  
        <!-- Detail Buku -->
        <h2 class="text-2xl font-semibold text-center mb-4">{{ book.judul }}</h2>
        <div class="mb-4">
          <p><strong>Pengarang:</strong> {{ book.pengarang }}</p>
          <p><strong>Tahun Terbit:</strong> {{ book.tahun }}</p>
          <p><strong>Jumlah Buku:</strong> {{ book.jumlah }}</p>
          <p><strong>Jumlah Dipinjam:</strong> {{ book.pinjam }}</p>
        </div>
  
        <!-- Peminjaman dan Pengembalian -->
        <div class="flex flex-col space-y-6">
          <!-- Peminjaman -->
          <div>
            <h3 class="text-lg font-semibold">Peminjaman Buku</h3>
            <div class="flex items-center space-x-4">
              <!-- Input number untuk peminjaman -->
              <input
                v-model="pinjamAmount"
                type="number"
                class="w-16 p-2 border rounded text-center"
                :max="book.jumlah"
                :min="0"
                required
                @input="validatePinjamAmount"
              />
              <button
                @click="borrowBook"
                :disabled="pinjamAmount === 0"
                class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
              >
                Pinjam Buku
              </button>
            </div>
          </div>
  
          <!-- Pengembalian -->
          <div>
            <h3 class="text-lg font-semibold">Pengembalian Buku</h3>
            <div class="flex items-center space-x-4">
              <!-- Input number untuk pengembalian -->
              <input
                v-model="returnAmount"
                type="number"
                class="w-16 p-2 border rounded text-center"
                :max="initialPinjam"
                :min="0"
                required
                @input="validateReturnAmount"
              />
              <button
                @click="returnBook"
                :disabled="returnAmount === 0"
                class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
              >
                Kembalikan Buku
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      showModal: Boolean,
      book: Object,
      closeModal: Function,
      updateBookList: Function,
    },
    data() {
      return {
        pinjamAmount: 0, // Jumlah buku yang ingin dipinjam
        returnAmount: 0, // Jumlah buku yang ingin dikembalikan
        initialPinjam: 0, // Melacak jumlah buku yang dipinjam sebelumnya
      };
    },
    methods: {
      // Fungsi untuk meminjam buku
      borrowBook() {
        if (this.pinjamAmount > 0 && this.pinjamAmount <= this.book.jumlah) {
          this.book.pinjam += this.pinjamAmount;
          this.initialPinjam += this.pinjamAmount; // Tambahkan ke jumlah pinjaman awal
          this.book.jumlah -= this.pinjamAmount;
          this.updateBookList(this.book); // Memperbarui data buku di daftar
          this.pinjamAmount = 0; // Reset input
        }
      },
      // Fungsi untuk mengembalikan buku
      returnBook() {
        if (this.returnAmount > 0 && this.returnAmount <= this.initialPinjam) {
          this.book.pinjam -= this.returnAmount;
          this.book.jumlah += this.returnAmount;
          this.initialPinjam -= this.returnAmount; // Kurangi jumlah pinjaman awal
          this.updateBookList(this.book); // Memperbarui data buku di daftar
          this.returnAmount = 0; // Reset input
        }
      },
      // Validasi untuk input peminjaman
      validatePinjamAmount() {
        if (this.pinjamAmount < 0) {
          this.pinjamAmount = 0;
        }
        if (this.pinjamAmount > this.book.jumlah) {
          this.pinjamAmount = this.book.jumlah;
        }
      },
      // Validasi untuk input pengembalian
      validateReturnAmount() {
        if (this.returnAmount < 0) {
          this.returnAmount = 0;
        }
        if (this.returnAmount > this.initialPinjam) {
          this.returnAmount = this.initialPinjam;
        }
      },
    },
  };
  </script>
  
  <style scoped>
  button:disabled {
    cursor: not-allowed;
    opacity: 0.5;
  }
  button {
    transition: background-color 0.3s ease;
  }
  </style>
  