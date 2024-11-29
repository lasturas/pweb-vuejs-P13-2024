<template>
  <div class="bg-white p-6 rounded-lg shadow-md mb-6">
    <h2 class="text-xl font-semibold mb-4">
      {{ editIndex !== null ? "Edit Buku" : "Tambah Buku" }}
    </h2>
    <form @submit.prevent="saveBook">
      <div class="mb-4">
        <label class="block text-gray-700 font-medium">Judul</label>
        <input
          v-model="form.judul"
          type="text"
          class="w-full p-2 border rounded"
          placeholder="Masukkan judul buku"
          required
          @input="validateJudul"
        />
        <p v-if="judulError" class="text-red-500 text-sm">Judul tidak boleh mengandung angka.</p>
      </div>
      <div class="mb-4">
        <label class="block text-gray-700 font-medium">Pengarang</label>
        <input
          v-model="form.pengarang"
          type="text"
          class="w-full p-2 border rounded"
          placeholder="Masukkan nama pengarang"
          required
          @input="validatePengarang"
        />
        <p v-if="pengarangError" class="text-red-500 text-sm">Pengarang tidak boleh mengandung angka.</p>
      </div>
      <div class="mb-4">
        <label class="block text-gray-700 font-medium">Tahun Terbit</label>
        <input
          v-model="form.tahun"
          type="number"
          class="w-full p-2 border rounded"
          placeholder="Masukkan tahun terbit"
          required
          @input="validateTahun"
        />
        <p v-if="tahunError" class="text-red-500 text-sm">Tahun harus berupa 4 digit angka dan tidak 0000.</p>
      </div>
      <div class="mb-4">
        <label class="block text-gray-700 font-medium">Jumlah Buku</label>
        <input
          v-model="form.jumlah"
          type="number"
          class="w-full p-2 border rounded"
          placeholder="Masukkan jumlah buku"
          required
          @input="validateJumlah"
        />
        <p v-if="jumlahError" class="text-red-500 text-sm">Jumlah buku harus lebih dari 0 dan tidak boleh mengandung huruf.</p>
      </div>
      <button
        type="submit"
        class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
        :disabled="judulError || pengarangError || tahunError || jumlahError"
      >
        {{ editIndex !== null ? "Simpan Perubahan" : "Tambah Buku" }}
      </button>
    </form>
  </div>
</template>

<script>
export default {
  props: {
    form: Object,
    editIndex: Number,
    saveBook: Function,
  },
  data() {
    return {
      judulError: false,
      pengarangError: false,
      tahunError: false,
      jumlahError: false,
    };
  },
  methods: {
    // Validasi untuk memastikan judul tidak mengandung angka
    validateJudul() {
      const judul = this.form.judul;
      this.judulError = /\d/.test(judul);  // Cek apakah ada angka dalam judul
    },
    // Validasi untuk memastikan pengarang tidak mengandung angka
    validatePengarang() {
      const pengarang = this.form.pengarang;
      this.pengarangError = /\d/.test(pengarang);  // Cek apakah ada angka dalam nama pengarang
    },
    // Validasi tahun terbit agar hanya 4 digit dan tidak 0000
    validateTahun() {
      const tahun = this.form.tahun;
      this.tahunError = !/^\d{4}$/.test(tahun) || tahun === '0000';
    },
    // Validasi jumlah buku agar tidak kurang dari 1 dan tidak mengandung huruf
    validateJumlah() {
      const jumlah = this.form.jumlah;
      this.jumlahError = jumlah <= 0 || isNaN(jumlah);
    }
  }
};
</script>
