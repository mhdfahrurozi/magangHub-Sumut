<template>
  <div class="font-sans min-h-screen bg-white">

    <header class="bg-linear-to-r from-blue-600 to-cyan-500 py-20 text-white flex flex-col items-center justify-center shadow-lg">
      <div class="w-full max-w-4xl px-4 text-center">
          <h1 class="text-4xl font-bold mb-2">Lowongan Magang di Sumatera Utara</h1>
          <p class="text-lg mb-8">Jelajahi berbagai lowongan magang yang sesuai dengan minat dan keahlianmu ~by Mhd Fahru Rozi</p>
      </div>
      

      <div class="w-full max-w-4xl px-4 mb-10">
        <div class="bg-white rounded-lg shadow-xl p-3 flex flex-col md:flex-row space-y-3 md:space-y-0 md:space-x-3 ">
          
          <div class="grow flex items-center border border-gray-300 rounded-lg px-4 py-2 focus-within:ring-2 focus-within:ring-cyan-5D00 focus-within:border-cyan-500 
              transition duration-150">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400 mr-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
            </svg>
            <input 
              type="text" 
              placeholder="Cari Posisi/Nama Perusahaan" 
              class="w-full bg-transparent focus:outline-none text-gray-700"
              v-model="keyword"
              @input="searchJob"
            >
          </div>
        </div>

      </div>
      <p class="text-base sm:text-lg font-bold">{{ loading ? 'Sedang mengambil data... harap menunggu ^_^' : 'Data berhasil di load semua ✅' }}</p>

    </header>

    <main class="py-10 px-4 sm:px-6 lg:px-8 max-w-7xl mx-auto">
      
     <h2 class="text-xl font-semibold text-gray-800 mb-6">Daftar Lowongan MagangHub</h2>
      
      <div class="flex flex-col sm:flex-row flex-wrap justify-between items-start sm:items-center mb-8 space-y-4 sm:space-y-0">
        
        <div class="flex flex-wrap items-center space-x-4 mb-2">
          <p class="text-gray-500 font-medium">Ditemukan {{ totalJob }} lowongan</p>



          <div class="relative min-w-[150px]">
            <select 
               v-model="sortJobs"
               @change="sortData"
               class="block w-full py-2 pl-3 pr-10 border border-gray-300 bg-white rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500 sm:text-sm appearance-none">
              <option value="newest">Terbaru</option>
              <option value="oldest">Terlama</option>
            </select>
            <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 9l4-4 4 4m0 6l-4 4-4-4" />
              </svg>
            </div>
          </div>
          <div class="relative min-w-[150px]">
            <select 
               v-model="sortPags"
               @change="sortPagination"
               class="block w-full py-2 pl-3 pr-10 border border-gray-300 bg-white rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500 sm:text-sm appearance-none">
              <option value="semua">Semua</option>
              <option value="20">20 Data</option>
              <option value="50">50 Data</option>
              <option value="100">100 Data</option>
            </select>
            <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 9l4-4 4 4m0 6l-4 4-4-4" />
              </svg>
            </div>
          </div>
        </div>

        <div class="flex space-x-3">
          <button 
            :disabled=loading
            @click="exportToJson()"
            :class="[
              'flex items-center px-4 py-2 border border-gray-400 text-sm font-medium rounded-md text-gray-700 bg-white transition duration-150',
              { 'hover:bg-gray-200': !loading }
            ]">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2 text-blue-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 17v-2m3 2v-4m3 4v-6m2 10H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" />
            </svg>
            Export JSON
          </button>
          
          <button 
            :disabled=loading
            @click="exportToExcel()"
            :class="[
              'flex items-center px-4 py-2 border border-green-500 text-sm font-medium rounded-md text-white bg-green-600',
              { 'hover:bg-green-700 transition duration-150' : !loading}
              ]">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 21h10a2 2 0 002-2V9.414a1 1 0 00-.293-.707l-5.414-5.414a1 1 0 00-.707-.293H7a2 2 0 00-2 2v14a2 2 0 002 2zM9 15h6m-6 4h6" />
            </svg>
            Export XLSX
          </button>

        </div>
      </div>

<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
    
    <div 
        v-for="(job, index) in paginatedJobs" 
        :key="index" 
        class="bg-white rounded-2xl shadow-xl border border-blue-200 hover:shadow-2xl hover:-translate-y-1 transition duration-300 overflow-hidden relative group"
    >
      <a :href="url+job.id_posisi" target="_blank" class="block h-full">
        
        <div class="absolute top-0 right-0 m-3 px-3 py-1 text-white text-xs font-bold rounded-full shadow-md"
            :class="getOpportunityColorClass(job.jumlah_terdaftar, job.jumlah_kuota)">
          Peluang : {{ getOpportunityPercentage(job.jumlah_terdaftar, job.jumlah_kuota) }}%
        </div>

        <div class="p-5">
          <div class="mb-4 pt-6">
            <p class="text-lg font-bold text-gray-800 leading-snug group-hover:text-blue-600 transition duration-150">{{ job.posisi }}</p>
            <p class="text-sm text-gray-600 mt-1">{{ job.perusahaan.nama_perusahaan }}</p>
          </div>

          <p class="text-xs text-gray-400 mb-4 flex items-center">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-red-400 mr-1.5" viewBox="0 0 20 20" fill="currentColor">
              <path fill-rule="evenodd" d="M5.05 4.05a7 7 0 119.9 9.9L10 18.9l-4.95-4.95a7 7 0 010-9.9zM10 11a2 2 0 100-4 2 2 0 000 4z" clip-rule="evenodd" />
            </svg>
            {{ job.perusahaan.nama_kabupaten }}, {{ job.perusahaan.nama_provinsi }}
          </p>
          
          <hr class="my-3 border-gray-100">

          <div class="flex flex-col space-y-2 text-sm text-gray-500">
            <div class="flex items-center">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-gray-400 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
              </svg>
              <span class="text-xs">{{ job.created_at }}</span>
            </div>
            <div class="flex items-center">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-gray-400 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
              </svg>
              <span class="text-xs font-semibold">{{ job.jumlah_terdaftar }} pelamar</span>
              <span class="text-xs mx-1 text-gray-300">|</span>
              <span class="text-xs">{{ job.jumlah_kuota }} kebutuhan</span>
            </div>
          </div>
        </div>
      </a>
    </div>

    </div>

    <div class="flex items-center justify-between border-t border-gray-200 bg-white px-4 py-3 sm:px-6">
      <div class="flex flex-1 justify-between sm:hidden">
        <a href="#" class="relative inline-flex items-center rounded-md border border-gray-300 bg-white px-4 py-2 text-sm font-medium text-gray-700 hover:bg-gray-50">Previous</a>
        <a href="#" class="relative ml-3 inline-flex items-center rounded-md border border-gray-300 bg-white px-4 py-2 text-sm font-medium text-gray-700 hover:bg-gray-50">Next</a>
      </div>
      <div class="hidden sm:flex sm:flex-1 sm:items-center sm:justify-between">
        <div>
          <p class="text-sm text-gray-700">
            Showing
            <span class="font-medium">{{ startItemIndex }}</span>
            to
            <span class="font-medium">{{ endItemIndex }}</span>
            of
            <span class="font-medium">{{ totalJob }}</span>
            results
          </p>
        </div>
        <div v-show="!loading">
          <nav aria-label="Pagination" class="isolate inline-flex -space-x-px rounded-md shadow-xs">
            <!-- Previous Button -->
            <button
              @click="prevPage"
              :disabled="currentPage === 1"
              class="relative inline-flex items-center rounded-l-md px-2 py-2 text-gray-400 inset-ring inset-ring-gray-300
                hover:bg-gray-50 focus:z-20 focus:outline-offset-0 disabled:opacity-40 disabled:cursor-not-allowed"
            >
              <span class="sr-only">Previous</span>
              <svg viewBox="0 0 20 20" fill="currentColor" class="size-5">
                <path d="M11.78 5.22a.75.75 0 0 1 0 1.06L8.06 10l3.72 3.72a.75.75 0 1 1-1.06 1.06l-4.25-4.25a.75.75 0 0 1 0-1.06l4.25-4.25a.75.75 0 0 1 1.06 0Z" />
              </svg>
            </button>

            <!-- Numbered buttons -->
            <template v-for="(page, index) in paginatedPages" :key="index">
              <span 
                  v-if="page === '...'"
                  class="relative inline-flex items-center px-4 py-2 text-sm font-semibold text-gray-700 inset-ring inset-ring-gray-300"
                  aria-hidden="true"
              >
                  {{ page }}
              </span>

              <button
                  v-else
                  @click="goToPage(page)"
                  :class="[
                      'relative inline-flex items-center px-4 py-2 text-sm font-semibold inset-ring inset-ring-gray-300 focus:z-20 focus:outline-offset-0',
                      page === currentPage
                          ? 'z-10 bg-indigo-600 text-white focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600'
                          : 'text-gray-900 hover:bg-gray-50'
                  ]"
              >
                  {{ page }}
              </button>
          </template>

            <!-- Next Button -->
            <button
              @click="nextPage"
              :disabled="currentPage === totalPages"
              class="relative inline-flex items-center rounded-r-md px-2 py-2 text-gray-400 inset-ring inset-ring-gray-300
                hover:bg-gray-50 focus:z-20 focus:outline-offset-0 disabled:opacity-40 disabled:cursor-not-allowed"
            >
              <span class="sr-only">Next</span>
              <svg viewBox="0 0 20 20" fill="currentColor" class="size-5">
                <path d="M8.22 5.22a.75.75 0 0 1 1.06 0l4.25 4.25a.75.75 0 0 1 0 1.06l-4.25 4.25a.75.75 0 0 1-1.06-1.06L11.94 10 8.22 6.28a.75.75 0 0 1 0-1.06Z" />
              </svg>
            </button>

          </nav>
        </div>
      </div>
    </div>


    </main>
  </div>
</template>

<script>
// import { ref } from "vue";
import axios from "axios";
import { toRaw } from "vue";
import * as XLSX from "xlsx";
import { saveAs } from "file-saver";

export default {
  data(){
   return{
      jobs: [],
      totalJob: '',
      loading: true,
      url: 'https://maganghub.kemnaker.go.id/lowongan/view/',
      keyword: '',
      filteredJobs: [],
      sortJobs: 'newest',
      sortPags: 'semua',

      currentPage: 1,
      perPage: 1263,
      paginatedJobs: [],
   }
  }, computed: {
    totalPages() {
      return Math.ceil(this.filteredJobs.length / this.perPage);
    },
    startItemIndex() {
      // Menghitung indeks data awal yang ditampilkan.
      // Jika perPage adalah "semua" (perPage = filteredJobs.length), ini akan kembali ke 1.
      if (this.perPage === this.filteredJobs.length) {
          return 1;
      }
      
      // Rumus standar: (currentPage - 1) * perPage + 1
      const start = (this.currentPage - 1) * this.perPage + 1;
      
      // Pastikan tidak lebih dari total data jika data kosong
      return this.filteredJobs.length === 0 ? 0 : start;
    },
    endItemIndex() {
      if (this.filteredJobs.length === 0) {
        return 0;
      }
      // Menghitung indeks data akhir yang ditampilkan.
      const end = this.currentPage * this.perPage;
      
      // Gunakan nilai yang lebih kecil antara hasil perhitungan atau total jumlah data yang difilter.
      return Math.min(end, this.filteredJobs.length);
    },
    paginatedPages(){
      const total = this.totalPages;
      const current = this.currentPage;
      const maxButtons = 7; // Jumlah maksimum tombol yang ditampilkan (termasuk tombol awal/akhir)

      if (total <= maxButtons + 2) { // Jika total halaman kecil, tampilkan semua
        return Array.from({ length: total }, (_, i) => i + 1);
      }

      let pages = [];
      const sideButtons = Math.floor(maxButtons / 2); // Jumlah tombol di sisi kiri/kanan halaman aktif

      // Tombol awal: [1], [2]...
      if (current <= maxButtons - 1) {
        for (let i = 1; i <= maxButtons; i++) {
          pages.push(i);
        }
        pages.push('...');
        pages.push(total);
      } 
      // Tombol akhir: ...[98], [99]
      else if (current >= total - maxButtons + 2) {
        pages.push(1);
        pages.push('...');
        for (let i = total - maxButtons + 1; i <= total; i++) {
          pages.push(i);
        }
      } 
      // Tombol di tengah: [1],...[4],[5],[6],...[99]
      else {
        pages.push(1);
        pages.push('...');
        for (let i = current - sideButtons + 1; i <= current + sideButtons; i++) {
          pages.push(i);
        }
        pages.push('...');
        pages.push(total);
      }
      
      // Filter untuk menghindari duplikat dan pastikan '...' tidak bersebelahan dengan angka
      return pages.filter((item, index, arr) => {
          if (item === '...' && (arr[index - 1] === '...' || arr[index + 1] === '...')) {
              return false;
          }
          return true;
      });
    }
  },
   mounted() {
   this.getData()
  }, methods: {
    async getData() {
      const baseUrl = "https://maganghub.kemnaker.go.id/be/v1/api/list/vacancies-aktif";
      const limit = 20;
      const kodeProvinsi = 12;

      let buffer = [];
      let page = 1;

      console.log("⏳ Mengambil semua data...");

      try {
        while (true) {
          const url = `${baseUrl}?order_by=created_at&order_direction=DESC&page=${page}&limit=${limit}&kode_provinsi=${kodeProvinsi}`;


          // timeout 10 detik supaya cepat fallback kalau API bermasalah
          const res = await axios.get(url, { timeout: 10000 });
          const data = res.data?.data || [];

          if (data.length === 0) break;
          buffer.push(...data);

          if (page % 5 === 0) {
            // Urutkan berdasarkan created_at descending
            buffer.sort((a, b) => new Date(b.created_at) - new Date(a.created_at));

            // Gabungkan dengan data lama
            this.jobs.push(...buffer);
            this.filteredJobs = this.jobs;

            // Kosongkan buffer
            buffer = [];

            // Force render update UI
            await this.$nextTick();

          }

          page++;
        }

        if (buffer.length > 0) {
          buffer.sort((a, b) => new Date(b.created_at) - new Date(a.created_at));
          this.jobs.push(...buffer);
          this.filteredJobs = this.jobs;
        }

        if (this.jobs.length === 0) {
          throw new Error("Data kosong, fallback ke backup JSON");
        }
        console.log(`🎉 Total data: ${this.jobs.length}`);
      } catch (err) {
        console.error("❌ Gagal mengambil data:", err);
        console.warn("🔁 Menggunakan data cadangan dari jobs_backup.json...");
          // try fallback data
          try {
            const backupRes = await fetch("/jobs_backup.json");
            if (!backupRes.ok) throw new Error("Gagal memuat file backup JSON");
            const backupData = await backupRes.json();

            this.jobs = backupData.sort(
              (a, b) => new Date(b.created_at) - new Date(a.created_at)
            );
            this.filteredJobs = this.jobs;

            console.log(`📦 Berhasil memuat ${this.jobs.length} data dari backup JSON`);
          } catch (backupErr) {
            console.error("❌ Gagal memuat data backup:", backupErr.message);
          }
          // end fallback data

      } finally {
        this.loading = false;
        this.totalJob = this.jobs.length;
        this.updatePagination();

      }
    },
    searchJob(){
      this.filteredJobs = this.jobs.filter(job => job.posisi.toLowerCase().includes(this.keyword.toLowerCase()) || job.perusahaan.nama_perusahaan.toLowerCase().includes(this.keyword.toLowerCase()))
      this.currentPage = 1;
      this.updatePagination();
    },
    sortData(){
      console.log("urutkan berdasarkan: ", this.sortJobs)

      if(this.sortJobs == 'oldest'){
         this.filteredJobs.sort((a, b) => new Date(a.created_at) - new Date(b.created_at));
      }

      if(this.sortJobs == 'newest'){
         this.filteredJobs.sort((a, b) => new Date(b.created_at) - new Date(a.created_at));
      }
      
      this.currentPage = 1;
      this.updatePagination();

    },
    exportToJson(){
       // Simpan backup ke file JSON
      const rawData = toRaw(this.jobs);
      const blob = new Blob([JSON.stringify(rawData, null, 2)], {
        type: "application/json",
      });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.download = "MagangHub-Batch2(sumut).json";
      a.click();
    },
    async exportToExcel() {
      if (!this.jobs.length) {
        alert("Data belum tersedia.");
        return;
      }

      // 1️⃣ Flatten / Normalisasi struktur data agar tidak nested
      const exportData = this.jobs.map((job) => ({
        ID_Posisi: job.id_posisi,
        Posisi: job.posisi,
        Deskripsi: job.deskripsi_posisi,
        Jenjang: Array.isArray(job.jenjang) ? job.jenjang.join(", ") : job.jenjang,
        Program_Studi: (() => {
          try {
            const ps = JSON.parse(job.program_studi);
            return ps.map((p) => p.title).join(", ");
          } catch {
            return "";
          }
        })(),
        Kuota: job.jumlah_kuota ?? "-",
        Terdaftar: job.jumlah_terdaftar ?? "-",
        Status_Posisi: job.ref_status_posisi?.nama_status_posisi ?? "-",
        Perusahaan: job.perusahaan?.nama_perusahaan ?? "-",
        Alamat: job.perusahaan?.alamat ?? "-",
        Kabupaten: job.perusahaan?.nama_kabupaten ?? "-",
        Provinsi: job.perusahaan?.nama_provinsi ?? "-",
        Instansi_Pemerintah: job.government_agency?.government_agency_name ?? "-",
        Sub_Instansi: job.sub_government_agency?.sub_government_agency_name ?? "-",
        Tanggal_Mulai: job.jadwal?.tanggal_mulai ?? "-",
        Tanggal_Selesai: job.jadwal?.tanggal_selesai ?? "-",
        Created_At: job.created_at,
        Updated_At: job.updated_at,
      }));

      // 2️⃣ Buat worksheet & workbook
      const worksheet = XLSX.utils.json_to_sheet(exportData);
      const workbook = XLSX.utils.book_new();
      XLSX.utils.book_append_sheet(workbook, worksheet, "Lowongan");

      // 3️⃣ Styling
      worksheet["!cols"] = [
        { wch: 10 }, // ID
        { wch: 20 }, // Posisi
        { wch: 50 }, // Deskripsi
        { wch: 15 }, // Jenjang
        { wch: 25 }, // Program Studi
        { wch: 10 }, // Kuota
        { wch: 10 }, // Terdaftar
        { wch: 20 }, // Status
        { wch: 25 }, // Perusahaan
        { wch: 40 }, // Alamat
        { wch: 20 }, // Kabupaten
        { wch: 20 }, // Provinsi
        { wch: 25 }, // Instansi
        { wch: 25 }, // Sub Instansi
        { wch: 20 }, // Tgl Mulai
        { wch: 20 }, // Tgl Selesai
        { wch: 25 }, // Created At
        { wch: 25 }, // Updated At
      ];

      // 4️⃣ Export ke file Excel
      const excelBuffer = XLSX.write(workbook, { bookType: "xlsx", type: "array" });
      const blob = new Blob([excelBuffer], {
        type: "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet",
      });
      saveAs(blob, `lowongan_maganghub_${new Date().toISOString().split("T")[0]}.xlsx`);
      console.log("✅ File Excel berhasil dibuat!");
    },
    sortPagination() {
      if (this.sortPags === "semua") {
        this.perPage = this.filteredJobs.length;
      } else {
        this.perPage = Number(this.sortPags);
      }
      this.currentPage = 1;
      this.updatePagination();
    },
    updatePagination() {
      const start = (this.currentPage - 1) * this.perPage;
      const end = start + this.perPage;
      this.paginatedJobs = this.filteredJobs.slice(start, end);
    },
    goToPage(page) {
      if (page < 1 || page > this.totalPages) return;
      this.currentPage = page;
      this.updatePagination();
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
        this.updatePagination();
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
        this.updatePagination();
      }
    },



    getOpportunityPercentage(terdaftar, kuota) {
      // Hitung persentase: (kuota / terdaftar) * 100
      let percentage = terdaftar > 0 ? (kuota / terdaftar) * 100 : 100;
      // Batasi agar tidak lebih dari 100%
      return Math.min(percentage, 100).toFixed(2);
    },
    getOpportunityColorClass(terdaftar, kuota) {
      const percentage = this.getOpportunityPercentage(terdaftar, kuota);
      const value = parseFloat(percentage);
      
      // 1. Peluang Rendah (Kurang dari 10%)
      if (value < 10) {
        return 'bg-red-600'; 
      }

      // 2. Peluang Rendah (Kurang dari 30%)
      if (value < 30) {
        return 'bg-blue-400'; 
      }
      
      // 3. Peluang Tinggi (70% ke atas)
      if (value >= 80) {
        return 'bg-blue-900'; 
      }

      // 4. Peluang Sedang (30% sampai < 70%)
      return 'bg-blue-600';
    }

  },
}
</script>