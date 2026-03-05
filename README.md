Berikut adalah draf README untuk workflow **Microstock Agent** Anda, disusun secara ringkas berdasarkan struktur node yang ada di dalam file:

---

# Microstock Agent Workflow

Workflow n8n ini dirancang untuk mengotomatisasi proses pembuatan aset gambar stok, mulai dari analisis visual, pembuatan prompt AI, hingga pengunggahan hasil akhir dan metadata ke Google Drive.

## Fitur Utama

* **Analisis Gambar Otomatis**: Menggunakan Google Gemini untuk menganalisis komposisi visual, palet warna, dan gaya artistik dari referensi gambar.
* **Generator Prompt AI**: Menghasilkan prompt deskriptif dalam bahasa Inggris yang dioptimalkan untuk model generatif seperti Midjourney atau Stable Diffusion melalui AI Agent.
* **Image Upscaler**: Terintegrasi dengan API eksternal untuk meningkatkan resolusi gambar secara otomatis sebelum disimpan.
* **Pembuat Metadata**: Menghasilkan judul, 45 kata kunci unik, dan kategori yang sesuai dengan standar **Adobe Stock** dan **Shutterstock**.
* **Manajemen Penyimpanan**: Mengatur folder di Google Drive berdasarkan tanggal dan secara otomatis memperbarui spreadsheet metadata.

## Alur Kerja (Workflow)

1. **Trigger**: Dimulai melalui pemicu manual (*Manual Trigger*).
2. **Analisis & Prompt**: Gambar dianalisis, lalu AI Agent membuat variasi prompt unik yang kemudian disimpan ke Google Sheets.
3. **Proses Gambar**: Gambar dikonversi ke format biner, ditingkatkan resolusinya (*upscaling*), dan diunduh kembali.
4. **Metadata & Upload**:
* AI Agent menghasilkan metadata spesifik untuk masing-masing platform (Adobe & Shutterstock).
* File gambar diunggah ke folder Google Drive yang terorganisir.
* Metadata dimasukkan ke dalam file Google Sheets (CSV) di dalam folder yang sama.



## Persyaratan (Prerequisites)

* **Credentials**:
* Google Sheets & Google Drive API.
* Google Gemini (PaLM) API.
* Akses API untuk layanan Upscaler (get1.imglarger.com).


* **Variabel**: Pastikan ID folder Google Drive dan ID Spreadsheet telah disesuaikan dengan lingkungan Anda.

---

*Catatan: Workflow ini menggunakan sistem loop untuk memproses beberapa item sekaligus dan memastikan status setiap proses tercatat di Google Sheets.*
