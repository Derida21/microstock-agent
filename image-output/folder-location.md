### Struktur Folder

Hasil eksekusi workflow ini akan diatur dalam struktur folder sebagai berikut:

```text
.
├── 📁 images_output/             # Folder utama untuk menyimpan hasil render/upscale
│   └── 📁 [DD-MM-YYYY]/   # Folder otomatis berdasarkan tanggal eksekusi
│       ├── 📄 Modern luxury living room Saint Patricks Day interior design.jpg #example title
│       └── 📄 Adobe Stock Metadat.xlsx # File Google Sheet yang diekspor/sinkron
├── 📄 Microstock-Agent.json # File workflow n8n
└── 📄 README.md

```

### Penjelasan Output

1. **Gambar (JPG)**: File gambar yang telah melalui proses *upscaling* dan siap unggah. Penamaan file mengikuti format judul yang dihasilkan oleh AI.
2. **Metadata (Google Sheets/xlsx)**: Setiap folder harian berisi file spreadsheet yang mencakup:
* **Title**: Judul deskriptif (maksimal 180 kata).
* **Keywords**: 45 kata kunci unik yang dipisahkan koma.
* **Category**: Kode kategori numerik (Adobe Stock) dan Nama Kategori (Shutterstock).
* **Status**: Informasi apakah aset sudah siap atau memerlukan pengecekan ulang.

