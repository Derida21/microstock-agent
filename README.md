# Microstock Agent Workflow

Workflow n8n ini dirancang untuk mengotomatisasi proses pembuatan aset gambar stok, mulai dari analisis visual, pembuatan prompt AI, hingga pengunggahan hasil akhir dan metadata ke Google Drive. Proses ini dikontrol dan dipantau melalui file Google Sheets yang spesifik.

## Fitur Utama

* **Analisis Gambar Otomatis**: Menggunakan Google Gemini untuk menganalisis komposisi visual, palet warna, dan gaya artistik dari url referensi gambar.
* **Generator Prompt AI**: Menghasilkan prompt deskriptif dalam bahasa Inggris yang dioptimalkan untuk model generatif seperti Midjourney atau Stable Diffusion melalui AI Agent.
* **Image Upscaler**: Terintegrasi dengan API eksternal untuk meningkatkan resolusi gambar secara otomatis sebelum disimpan.
* **Pembuat Metadata**: Menghasilkan judul, 45 kata kunci unik, dan kategori yang sesuai dengan standar **Adobe Stock** dan **Shutterstock**.
* **Manajemen Penyimpanan**: Mengatur folder di Google Drive berdasarkan tanggal dan secara otomatis memperbarui spreadsheet metadata.

## Alur Kerja (Workflow)

Workflow ini beroperasi dalam dua tahap utama yang dikendalikan oleh dua *sheet* berbeda:

### Tahap 1: Kontrol Input dan Pembuatan Prompt (*Sheet Starter*)

Tahap ini menggunakan *sheet* bernama **`starter`** sebagai instruksi manual untuk workflow.

1. **Trigger**: Workflow dimulai melalui pemicu manual (*Manual Trigger*).
2. **Membaca Instruksi**: Workflow membaca data input dari *sheet* `starter`, yang mencakup:
* **`url_reference`**: URL gambar referensi untuk dianalisis.
* **`Image_ratio`**: Rasio aspek yang diinginkan untuk gambar yang dihasilkan (terdapat menu dropdown).
* **`text_plan`**: Instruksi teks tambahan atau konteks untuk memandu pembuatan prompt.
* **`count`**: Jumlah variasi prompt unik yang ingin dihasilkan.


3. **Analisis & Pembuatan Prompt**: Berdasarkan input ini, AI Agent menganalisis gambar referensi dan instruksi teks untuk membuat jumlah prompt unik yang ditentukan.
4. **Menyimpan Prompt**: Prompt yang dihasilkan kemudian disimpan ke dalam *sheet* **`generated-prompt`**, dan statusnya awalnya disetel ke "PROCESS".

### Tahap 2: Pemrosesan Gambar dan Metadata (*Sheet Generated-Prompt*)

Tahap ini menggunakan *sheet* bernama **`generated-prompt`** untuk melakukan *looping* dan memproses setiap prompt yang dihasilkan secara terpisah.

1. **Membaca Prompt untuk Looping**: Workflow mengambil data dari *sheet* `generated-prompt`, yang berisi:
* **`prompt`**: Prompt teks yang siap digunakan untuk menghasilkan gambar.
* **`status`**: Menunjukkan status pemrosesan prompt (misalnya, PROCESS, SUCCES).


2. **Pemrosesan Per Prompt (Loop)**: Workflow melakukan *looping* melalui setiap baris di *sheet* `generated-prompt` yang memiliki status "PROCESS". Untuk setiap prompt:
* **Hasilkan Metadata**: AI Agent menghasilkan metadata spesifik (judul, kata kunci, kategori) untuk Adobe Stock dan Shutterstock berdasarkan deskripsi prompt.
* **Hasilkan & Tingkatkan Resolusi Gambar**: Gambar dihasilkan menggunakan prompt, kemudian resolusinya ditingkatkan (*upscaling*).
* **Unggah ke Google Drive**: File gambar yang ditingkatkan resolusinya diunggah ke folder Google Drive yang terorganisir berdasarkan tanggal.
* **Perbarui Metadata di Google Sheets**: Metadata yang dihasilkan ditambahkan ke file Google Sheets (CSV) di dalam folder Drive yang sama.
* **Perbarui Status Prompt**: Setelah pemrosesan selesai (sukses), status untuk prompt tersebut di *sheet* `generated-prompt` diperbarui menjadi "SUCCES".



## Struktur Direktori

Hasil eksekusi workflow ini akan diatur dalam struktur folder di Google Drive sebagai berikut:

```text
.
├── 📁 image_output/             # Folder utama untuk menyimpan hasil render/upscale
│   └── 📁/   # Folder otomatis berdasarkan tanggal eksekusi
│       ├── 📄 image_01.jpg
│       ├── 📄 image_02.jpg
│       ├── 📄 Adobe Stock Metadata.csv # File metadata untuk Adobe Stock
│       └── 📄 Shutter Stock Metadata.csv # File metadata untuk Shutterstock
├── 📄 Microstock-Agent.json # File workflow n8n
├── 📄 Micrstock Starter.xlsx
└── 📄 README.md

```

## Persyaratan (Prerequisites)

* **Credentials**:
* Google Sheets & Google Drive API.
* Google Gemini (PaLM) API.
* Akses API untuk layanan Upscaler (get1.imglarger.com).


* **Variabel**: Pastikan ID folder Google Drive dan ID Spreadsheet telah disesuaikan dengan lingkungan Anda.
