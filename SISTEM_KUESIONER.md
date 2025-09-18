# Sistem Kuesioner Kepuasan Pengunjung

## Deskripsi
Sistem kuesioner kepuasan pengunjung adalah fitur yang memungkinkan pengunjung untuk memberikan feedback setelah selesai berkunjung. Sistem ini terintegrasi dengan sistem buku tamu yang sudah ada.

## Fitur Utama

### 1. Manajemen Pertanyaan (Admin)
- **Tambah Pertanyaan Baru**: Admin dapat menambahkan pertanyaan dengan berbagai tipe
- **Edit Pertanyaan**: Mengubah pertanyaan yang sudah ada
- **Hapus Pertanyaan**: Menghapus pertanyaan yang tidak diperlukan
- **Atur Urutan**: Mengatur urutan tampilan pertanyaan
- **Status Aktif/Nonaktif**: Mengontrol pertanyaan mana yang ditampilkan

### 2. Tipe Pertanyaan
- **Pilihan Ganda**: Pertanyaan dengan opsi jawaban yang telah ditentukan
- **Skala**: Pertanyaan dengan skor 1-10
- **Essai**: Pertanyaan terbuka untuk jawaban bebas

### 3. Pengisian Kuesioner (Pengunjung)
- **Progress Bar**: Menampilkan progress pengisian kuesioner
- **Validasi**: Memastikan pertanyaan wajib diisi
- **User-friendly**: Interface yang mudah digunakan
- **Responsive**: Dapat diakses dari berbagai perangkat

### 4. Analisis Jawaban (Admin)
- **Dashboard**: Overview statistik kuesioner
- **Detail Jawaban**: Melihat jawaban per pertanyaan
- **Export Data**: Export jawaban ke format CSV
- **Statistik**: Analisis jawaban terbanyak dan rata-rata skor

## Struktur Database

### Tabel: questionnaire_questions
```sql
- id (Primary Key)
- pertanyaan (TEXT)
- tipe_pertanyaan (ENUM: pilihan_ganda, skala, essai)
- opsi_jawaban (TEXT - JSON untuk pilihan ganda)
- urutan (INT)
- wajib_diisi (BOOLEAN)
- status (ENUM: aktif, nonaktif)
- created_at (DATETIME)
- updated_at (DATETIME)
```

### Tabel: questionnaire_answers
```sql
- id (Primary Key)
- buku_tamu_id (Foreign Key ke buku_tamu)
- question_id (Foreign Key ke questionnaire_questions)
- jawaban (TEXT)
- skor (INT - untuk pertanyaan skala)
- created_at (DATETIME)
```

## Cara Penggunaan

### Untuk Admin

1. **Akses Dashboard Kuesioner**
   ```
   URL: /admin/quesioner
   ```

2. **Kelola Pertanyaan**
   ```
   URL: /admin/quesioner/questions
   ```

3. **Tambah Pertanyaan Baru**
   ```
   URL: /admin/quesioner/create
   ```

4. **Lihat Jawaban**
   ```
   URL: /admin/quesioner/answers
   ```

5. **Export Data**
   ```
   URL: /admin/quesioner/export-answers
   ```

### Untuk Pengunjung

1. **Akses Kuesioner**
   ```
   URL: /questionnaire/{id_buku_tamu}
   ```

2. **Isi Kuesioner**
   - Jawab semua pertanyaan yang wajib diisi
   - Lihat progress bar untuk mengetahui kemajuan
   - Submit kuesioner

3. **Halaman Terima Kasih**
   ```
   URL: /questionnaire/thank-you
   ```

## Alur Kerja

1. **Pengunjung datang** → Daftar di buku tamu
2. **Pengunjung selesai** → Update status menjadi "Selesai"
3. **Admin dapat mengirim link kuesioner** → `/questionnaire/{id_buku_tamu}`
4. **Pengunjung mengisi kuesioner** → Submit jawaban
5. **Admin melihat hasil** → Dashboard dan analisis

## API Endpoints

### Admin Routes
```
GET  /admin/quesioner                    - Dashboard kuesioner
GET  /admin/quesioner/questions          - Kelola pertanyaan
GET  /admin/quesioner/create             - Form tambah pertanyaan
POST /admin/quesioner/store              - Simpan pertanyaan baru
GET  /admin/quesioner/edit/{id}          - Form edit pertanyaan
POST /admin/quesioner/update/{id}        - Update pertanyaan
GET  /admin/quesioner/delete/{id}        - Hapus pertanyaan
GET  /admin/quesioner/answers            - Lihat semua jawaban
GET  /admin/quesioner/answer-detail/{id} - Detail jawaban per pertanyaan
GET  /admin/quesioner/export-answers     - Export semua jawaban
GET  /admin/quesioner/export-answers/{id} - Export jawaban per pertanyaan
POST /admin/quesioner/reorder            - Atur urutan pertanyaan
```

### Public Routes
```
GET  /questionnaire/{id}                 - Form kuesioner pengunjung
POST /questionnaire/submit               - Submit jawaban kuesioner
GET  /questionnaire/thank-you            - Halaman terima kasih
GET  /questionnaire/preview/{id}         - Preview jawaban (opsional)
```

## Konfigurasi

### Menambahkan Pertanyaan Contoh
Jalankan seeder untuk menambahkan pertanyaan contoh:
```bash
php spark db:seed QuestionnaireSeeder
```

### Migration
Jalankan migration untuk membuat tabel:
```bash
php spark migrate
```

## Keamanan

1. **CSRF Protection**: Semua form dilindungi dengan CSRF token
2. **Validasi Input**: Validasi server-side untuk semua input
3. **Access Control**: Hanya admin yang dapat mengakses fitur manajemen
4. **Data Validation**: Validasi tipe pertanyaan dan jawaban
5. **Prevention of Duplicate**: Mencegah pengunjung mengisi kuesioner berulang

## Fitur Tambahan

### 1. Progress Tracking
- Progress bar real-time saat mengisi kuesioner
- Indikator pertanyaan yang sudah dijawab

### 2. Responsive Design
- Interface yang responsif untuk desktop dan mobile
- Optimized untuk berbagai ukuran layar

### 3. Export Functionality
- Export ke format CSV
- Support untuk export semua data atau per pertanyaan

### 4. Analytics Dashboard
- Statistik pertanyaan terpopuler
- Rata-rata skor kepuasan
- Grafik dan visualisasi data

## Troubleshooting

### Masalah Umum

1. **Pertanyaan tidak muncul**
   - Pastikan status pertanyaan "aktif"
   - Cek urutan pertanyaan

2. **Kuesioner tidak bisa diakses**
   - Pastikan ID buku tamu valid
   - Cek apakah sudah mengisi kuesioner sebelumnya

3. **Export tidak berfungsi**
   - Pastikan ada data jawaban
   - Cek permission folder writable

### Log dan Debug
- Cek log di `writable/logs/`
- Gunakan debug mode untuk troubleshooting

## Pengembangan Selanjutnya

1. **Notifikasi Email**: Kirim notifikasi ke admin saat ada jawaban baru
2. **Grafik Interaktif**: Tambahkan grafik dan chart untuk analisis
3. **Filter dan Pencarian**: Filter jawaban berdasarkan tanggal, pertanyaan, dll
4. **Multi-language**: Support untuk bahasa lain
5. **API Integration**: API untuk integrasi dengan sistem lain
6. **Mobile App**: Aplikasi mobile untuk pengisian kuesioner

## Kontribusi

Untuk berkontribusi pada pengembangan sistem ini:
1. Fork repository
2. Buat branch fitur baru
3. Commit perubahan
4. Push ke branch
5. Buat Pull Request

## Lisensi

Sistem ini menggunakan lisensi yang sama dengan proyek utama. 