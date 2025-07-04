# digital-product-marketplace-db

Repositori ini merupakan dokumentasi dan implementasi sistem basis data untuk marketplace produk digital.  
Proyek ini bertujuan untuk membangun fondasi database yang terstruktur, efisien, dan mudah dikembangkan untuk mendukung proses jual-beli produk digital secara online.

---

## Tentang Proyek

Marketplace produk digital membutuhkan sistem penyimpanan data yang andal agar transaksi, pengelolaan produk, dan aktivitas pengguna dapat berjalan lancar dan aman.  
Pada proyek ini, seluruh tahapan pengembangan database — mulai dari analisis kebutuhan, perancangan konseptual, logis, hingga implementasi fisik dan pengujian — didokumentasikan secara detail agar dapat menjadi referensi bersama, baik untuk pengembangan lebih lanjut maupun pembelajaran.

---

## Dokumentasi Per Fase

Setiap fase pengembangan disusun dalam file terpisah di folder [`docs/`](docs):

- **Fase 1 – Analisis dan Perancangan Konseptual**  
  Menjelaskan latar belakang, analisis kebutuhan, identifikasi entitas dan relasi, serta pembuatan ERD (Entity Relationship Diagram).  
  → [`docs/fase1_analisis_perancangan.md`](docs/fase1_analisis_perancangan.md)

- **Fase 2 – Perancangan Logis dan Normalisasi**  
  Meliputi transformasi ERD ke tabel relasional, proses normalisasi untuk menghilangkan redundansi, dan penentuan skema database secara logis maupun fisik.  
  → [`docs/fase2_normalisasi_database.md`](docs/fase2_normalisasi_Database.md)

- **Fase 3 – Implementasi Database & Query**  
  Berisi script pembuatan database (DDL), pengisian data sample (DML), berbagai query, view, trigger, hingga pengujian dan penjelasan narasi di setiap tahapan.  
  → [`docs/fase3_implementasi_database.md`](docs/fase3_implementasi_database.md)

> **Catatan:**  
> Laporan akhir (Fase 4) tersedia dalam format Word dan tidak dicantumkan di repositori ini.

---

## Struktur Folder

```
docs/         # Dokumentasi tiap fase (fase 1–3)
erd/          # Diagram ERD (gambar & file pendukung)
schema/       # Script DDL (CREATE TABLE, ALTER, dsb)
sample_data/  # Script DML (INSERT, UPDATE, DELETE)
queries/      # Query SQL (SELECT, VIEW, advanced, trigger, dsb)
images/       # Screenshot hasil pengujian/query
README.md     # Penjelasan utama repositori
LICENSE       # Lisensi (opsional)
.gitignore    # Pengaturan gitignore (opsional)
```

---

## Cara Menggunakan

1. **Setup Database:**  
   Jalankan script di folder `schema/` untuk membuat seluruh tabel.

2. **Isi Data Sample:**  
   Gunakan script pada folder `sample_data/` untuk mengisi data awal/simulasi.

3. **Uji Query & Fitur Lanjutan:**  
   Jalankan query yang tersedia di folder `queries/` untuk kebutuhan pengujian, analisis, atau pelaporan.

4. **Baca Dokumentasi:**  
   Pelajari setiap fase pengembangan di folder `docs/` untuk memahami alur perancangan sampai implementasi.

---

## Kontribusi

Kontributor sangat terbuka untuk melakukan fork, pull request, maupun diskusi pengembangan lebih lanjut.  
Silakan ajukan issue jika menemukan bug, kekurangan, atau ide baru untuk pengembangan sistem basis data marketplace ini.

---

© 2025 – Kelompok 6
