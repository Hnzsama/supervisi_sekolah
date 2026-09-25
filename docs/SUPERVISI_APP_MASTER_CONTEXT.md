# MASTER CONTEXT — SISTEM SUPERVISI GURU BERBASIS AI

> **Dokumen ini adalah konteks mentah/utama untuk AI coding agent.**
>
> Tujuannya bukan menjadi spesifikasi teknis final, melainkan menjelaskan **seluruh pemahaman tentang aplikasi yang diperoleh dari penjelasan awal pengguna dan catatan hasil percakapan dengan calon client**.
>
> Agent boleh memecah dokumen ini menjadi beberapa dokumentasi yang lebih kecil seperti product requirements, business workflow, database/domain model, permissions, AI architecture, Google Drive integration, observation system, reporting, dan implementation plan.
>
> **Penting:** Jangan mengubah asumsi menjadi requirement final. Bagian yang belum jelas harus ditandai sebagai `OPEN QUESTION`, `ASSUMPTION`, atau `DECISION REQUIRED`.

---

# 1. Gambaran Umum Produk

Aplikasi yang akan dibuat adalah sebuah **sistem supervisi akademik guru** yang digunakan oleh:

1. **Pengawas**
2. **Kepala Sekolah**
3. **Guru**

Tujuan utamanya adalah membantu proses supervisi guru dari awal sampai akhir, yaitu:

```text
Perencanaan
    ↓
Pemeriksaan Perangkat Pembelajaran
    ↓
Observasi/Pelaksanaan Pembelajaran
    ↓
Analisis Hasil
    ↓
Tindak Lanjut
    ↓
Laporan
```

Aplikasi menggunakan AI sebagai alat bantu untuk menganalisis perangkat pembelajaran, membantu membaca hasil observasi, dan memberikan rekomendasi tindak lanjut.

Namun berdasarkan requirement yang dipahami, **AI bukan pihak yang mengambil keputusan akhir**.

Hasil AI masih harus dapat:
- dilihat,
- dikroscek,
- dikoreksi,
- disetujui,
- atau ditolak

oleh Pengawas dan/atau Kepala Sekolah sesuai kewenangan mereka.

---

# 2. Konteks Awal dari Client

Dari komunikasi awal, client menjelaskan kebutuhan aplikasi untuk:

> menilai kinerja guru untuk supervisi, dengan pengguna Pengawas, Kepala Sekolah, dan Guru.

Aplikasi diharapkan membantu Pengawas dan Kepala Sekolah melihat kelengkapan perangkat pembelajaran guru, melakukan observasi pembelajaran di kelas, melakukan tindak lanjut, dan akhirnya menghasilkan laporan supervisi yang dapat diunduh.

Client juga menyebut adanya bantuan AI untuk:
- membaca/menganalisis perangkat pembelajaran,
- merekomendasikan informasi mengenai kelengkapan dan kualitas perangkat,
- menganalisis hasil observasi,
- dan merekomendasikan tindak lanjut.

---

# 3. Aktor / Role

## 3.1 Pengawas

Pengawas merupakan role dengan cakupan paling luas.

Satu Pengawas dapat memiliki beberapa sekolah yang menjadi tanggung jawabnya.

Contoh:

```text
Pengawas A
│
├── Sekolah 1
│   ├── Guru 1
│   ├── Guru 2
│   └── Guru 3
│
├── Sekolah 2
│   ├── Guru 1
│   ├── Guru 2
│   └── Guru 3
│
└── Sekolah 3
    ├── Guru 1
    └── Guru 2
```

Pengawas perlu dapat melihat kondisi supervisi secara agregat dari sekolah-sekolah yang menjadi tanggung jawabnya.

### Kebutuhan Pengawas yang dipahami

Pengawas dapat:
- melihat sekolah yang menjadi tanggung jawabnya,
- menambahkan sekolah secara manual,
- melihat guru pada sekolah,
- melihat kelengkapan perangkat pembelajaran guru,
- melihat hasil analisis AI,
- melakukan cross-check/verifikasi hasil AI,
- melakukan supervisi/observasi,
- membuat atau mengatur instrumen observasi,
- mengisi penilaian,
- memberikan catatan,
- melihat hasil observasi,
- melihat rekomendasi AI,
- melakukan koreksi terhadap rekomendasi,
- mengelola tindak lanjut,
- melihat laporan,
- dan mengunduh laporan.

### Penambahan sekolah oleh Pengawas

Dari penjelasan pengguna:

> "untuk pengawas untuk inputan data sekolah yang menjadi usernya di add manual untuk menambah sekolah user secara manual"

Pemahaman yang paling mungkin:

Pengawas mempunyai fitur untuk memasukkan sekolah yang menjadi bagian dari cakupan pengawasannya secara manual.

Namun detail apakah Pengawas:
- membuat akun sekolah,
- membuat akun Kepala Sekolah,
- hanya menambahkan data sekolah,
- atau mengundang Kepala Sekolah

masih perlu dikonfirmasi.

---

# 4. Kepala Sekolah

Kepala Sekolah memiliki cakupan yang lebih kecil dibanding Pengawas.

Jika Pengawas dapat melihat beberapa sekolah, Kepala Sekolah hanya berfokus pada sekolahnya sendiri.

Contoh:

```text
Kepala Sekolah A
│
└── Sekolah A
    ├── Guru 1
    ├── Guru 2
    ├── Guru 3
    ├── ...
    └── Guru 15
```

Client memberikan contoh:

> Jika ada 15 guru, dashboard Kepala Sekolah hanya menampilkan 15 guru tersebut.

Jadi dashboard Kepala Sekolah **tidak menampilkan sekolah lain**.

### Kebutuhan Kepala Sekolah yang dipahami

Kepala Sekolah dapat:
- melihat guru di sekolahnya,
- melihat kelengkapan perangkat guru,
- melihat analisis/rekomendasi AI,
- melakukan cross-check manual,
- melakukan observasi pembelajaran,
- menggunakan instrumen observasi,
- memberikan skor,
- memberikan catatan,
- melihat hasil observasi,
- melakukan tindak lanjut,
- memeriksa rekomendasi AI,
- dan menghasilkan/mengunduh laporan.

---

# 5. Guru

Guru merupakan pihak yang menyediakan perangkat pembelajaran dan menjadi objek supervisi.

Guru kemungkinan memiliki akun sendiri, tetapi mekanisme pembuatan akun masih perlu dikonfirmasi.

Guru mempunyai perangkat pembelajaran yang disimpan di Google Drive.

Contoh:

```text
Guru
│
└── Google Drive
    │
    └── Folder Supervisi
        ├── Rencana Pembelajaran
        ├── Modul Ajar
        ├── Bahan Ajar
        ├── LKPD
        ├── Asesmen
        └── Dokumen lainnya
```

Guru tidak harus mengunggah seluruh file langsung ke storage aplikasi.

Konsep yang disampaikan adalah:

```text
Guru menyimpan dokumen di Google Drive
        ↓
Google Drive ditautkan ke aplikasi
        ↓
Aplikasi dapat melihat/membaca dokumen
        ↓
Dokumen dapat dianalisis AI
        ↓
Pengawas/Kepala Sekolah dapat melihat hasilnya
```

---

# 6. Struktur Besar Aplikasi

Secara konsep, aplikasi memiliki beberapa bagian besar:

```text
Dashboard
│
├── Manajemen Sekolah
├── Manajemen Guru
├── Perencanaan Pembelajaran
├── Perangkat Pembelajaran
├── Google Drive
├── Analisis AI
├── Supervisi
│   ├── Instrumen
│   ├── Observasi
│   └── Hasil Observasi
├── Tindak Lanjut
└── Laporan
```

Tetapi secara alur bisnis:

```text
GURU
 │
 ├── menyediakan perangkat
 │
 ▼
GOOGLE DRIVE
 │
 ▼
ANALISIS PERANGKAT
 │
 ▼
VERIFIKASI PENGAWAS / KEPALA SEKOLAH
 │
 ▼
OBSERVASI PEMBELAJARAN
 │
 ▼
PENILAIAN + CATATAN
 │
 ▼
HASIL OBSERVASI
 │
 ▼
AI REKOMENDASI
 │
 ▼
TINDAK LANJUT
 │
 ▼
LAPORAN
```

---

# 7. Dashboard Pengawas

Dashboard Pengawas harus memberikan gambaran keseluruhan terhadap sekolah-sekolah yang diawasinya.

Misalnya Pengawas memiliki 7 sekolah.

Dashboard dapat menampilkan:

```text
7 Sekolah
105 Guru

Kelengkapan Perangkat
82%

Observasi
74%

Tindak Lanjut
61%
```

Kemudian daftar sekolah:

```text
Sekolah 1
15 Guru
Kelengkapan 87%

Sekolah 2
20 Guru
Kelengkapan 79%

Sekolah 3
18 Guru
Kelengkapan 91%

...
```

Client menyampaikan contoh:

> "misal ada 7 sekolah semua ditampilkan"

Artinya Pengawas membutuhkan **overview lintas sekolah**.

Angka persentase di atas hanyalah contoh untuk menjelaskan konsep, bukan nilai/kategori final.

---

# 8. Dashboard Kepala Sekolah

Dashboard Kepala Sekolah hanya mencakup sekolahnya sendiri.

Misalnya:

```text
Sekolah A

15 Guru

Kelengkapan Perangkat: 87%
Observasi: 73%
Tindak Lanjut: 55%
```

Kemudian daftar guru:

```text
Guru 1
Guru 2
Guru 3
...
Guru 15
```

Setiap guru dapat memiliki informasi:
- kelengkapan perangkat,
- hasil observasi,
- tindak lanjut,
- status supervisi.

---

# 9. Perencanaan Pembelajaran

Salah satu bagian penting aplikasi adalah **perencanaan pembelajaran/perangkat pembelajaran**.

Guru menyediakan perangkat pembelajaran.

Jenis perangkat final belum ditentukan secara pasti.

Contoh yang mungkin termasuk:
- Rencana Pembelajaran,
- Modul Ajar,
- Bahan Ajar,
- LKPD,
- Asesmen,
- Media Pembelajaran,
- perangkat lainnya.

**Daftar di atas masih contoh dan bukan daftar final.**

Sistem harus memungkinkan daftar perangkat ditentukan sesuai kebutuhan bisnis.

---

# 10. Integrasi Google Drive

Konsep yang disampaikan client:

> Guru meng-upload/menyediakan dokumen di Google Drive masing-masing, kemudian Google Drive ditautkan ke aplikasi supaya dapat dilihat oleh Pengawas dan Kepala Sekolah serta dianalisis oleh AI.

Alur konseptual:

```text
Guru
 ↓
Connect Google Drive
 ↓
Pilih folder/dokumen supervisi
 ↓
Aplikasi mendapatkan akses
 ↓
Aplikasi membaca metadata/dokumen
 ↓
AI menganalisis
```

Aplikasi perlu mengetahui dokumen yang tersedia.

Contoh:

```text
Folder Supervisi Guru

11 perangkat ditemukan
```

Kemudian aplikasi memetakan dokumen tersebut ke kebutuhan perangkat pembelajaran.

---

# 11. Analisis Perangkat oleh AI

Ini salah satu fitur utama aplikasi.

Client memberikan contoh:

> Jika ada 11 perangkat yang di-upload, AI merekomendasikan jumlah perangkat yang tersedia ke dashboard Pengawas dan Kepala Sekolah serta kualitasnya.

Pemahaman:

Misalnya sistem mengharapkan sejumlah jenis perangkat.

Guru menyediakan 11 dokumen.

AI kemudian dapat membantu menentukan:

```text
Jumlah dokumen ditemukan
Jumlah perangkat yang teridentifikasi
Perangkat yang belum ditemukan
Perangkat yang perlu ditinjau
Analisis kualitas
Rekomendasi
```

Contoh konseptual:

```text
Perangkat ditemukan: 11

Teridentifikasi:
9

Perlu pemeriksaan:
2

Perangkat belum ditemukan:
3
```

AI juga dapat memberikan analisis kualitas, misalnya:

```text
Perangkat ditemukan.

Analisis:
- bagian tertentu tersedia,
- bagian tertentu perlu ditinjau,
- ada komponen yang mungkin belum lengkap.
```

Namun AI **tidak boleh dianggap sebagai keputusan final**.

---

# 12. Human Cross-Check

Client secara eksplisit menyampaikan bahwa hasil AI nantinya:

> "nanti juga di cek oleh pengawas dan kepala sekolah di kroscek secara manual"

Artinya sistem wajib menyediakan proses:

```text
AI Analysis
     ↓
Human Review
     ↓
Accepted / Corrected / Rejected
```

Contoh:

```text
AI:
"Modul Ajar ditemukan."

Reviewer:
☑ Diverifikasi

Catatan:
"Dokumen sesuai."
```

Atau:

```text
AI:
"Perangkat asesmen ditemukan."

Reviewer:
☐ Tidak sesuai

Catatan:
"File tidak sesuai dengan perangkat yang dimaksud."
```

Hasil final harus membedakan:
- hasil AI,
- hasil review manusia.

---

# 13. Supervisi

Setelah tahap perangkat/perencanaan, terdapat tahap supervisi pembelajaran.

Supervisi memiliki bagian:

```text
1. Perencanaan
2. Observasi/Pelaksanaan Pembelajaran
3. Tindak Lanjut
4. Laporan
```

Client berharap aplikasi mencakup:

> rencana pembelajaran, observasi pembelajaran, tindak lanjut, laporan

---

# 14. Observasi Pembelajaran

Observasi dilakukan ketika Pengawas atau Kepala Sekolah melakukan kunjungan/pengamatan pembelajaran di kelas.

Aplikasi harus menyediakan form observasi.

Pengawas/Kepala Sekolah mengisi:
- instrumen,
- nilai,
- catatan.

---

# 15. Instrumen Observasi Harus Fleksibel

Client menyampaikan:

> "di pelaksanaan pembelajaran, diberikan kebebasan oleh 2 role, bisa menginput instrumen bisa diinput secara manual apa saja yang akan menjadi penilai kunjungan di dalam kelas"

Pemahaman:

Pengawas dan Kepala Sekolah tidak hanya menggunakan instrumen yang dibuat developer.

Mereka membutuhkan kemampuan untuk membuat/memasukkan instrumen penilaian sendiri.

Contoh:

```text
Instrumen:
Observasi Pembelajaran

Indikator:
1. ...
2. ...
3. ...
...
12. ...
```

Jumlah 12 hanya contoh dari pembicaraan, **bukan berarti sistem harus selalu memiliki tepat 12 indikator**.

Sistem sebaiknya configurable.

---

# 16. Pendekatan Pembelajaran Mendalam

Client menyebut:

> "pendekatan pembelajaran mendalam"

Ini berarti salah satu kemungkinan area yang ingin dinilai dalam observasi adalah pendekatan pembelajaran mendalam.

Namun definisi resmi indikatornya belum diberikan.

Jangan membuat indikator sendiri sebagai fakta.

Lebih aman menyediakan:

```text
Kategori/Domain:
Pembelajaran Mendalam

Indikator:
dapat dikonfigurasi
```

Isi indikator final harus berasal dari client/pemilik produk.

---

# 17. Skala Penilaian

Client menyampaikan:

> "instrumen 1-12 itu berbentuk skala 1-3 atau 4"

Pemahaman:

Setiap indikator dapat memiliki skala penilaian.

Contoh:

```text
Skala 1–4
1
2
3
4
```

atau:

```text
Skala 1–3
1
2
3
```

Sebaiknya sistem mendukung konfigurasi skala.

Jangan hard-code hanya 1–4 jika requirement memang ingin 1–3 atau 1–4.

---

# 18. Catatan per Indikator

Selain skor, setiap indikator perlu memiliki tempat untuk catatan.

Contoh:

```text
Indikator:
Guru melibatkan peserta didik secara aktif.

Nilai:
4

Catatan:
Peserta didik aktif berdiskusi.
```

Client menyebut:

> "di setiap itu ada tempat untuk mengisi catatan oleh 2 role itu"

Artinya Pengawas dan Kepala Sekolah dapat memberikan catatan pada hasil observasi sesuai kewenangannya.

---

# 19. Akumulasi Nilai Observasi

Setelah seluruh indikator diisi, hasil harus dihitung otomatis.

Contoh:

```text
12 indikator
Skala maksimal 4

Nilai maksimum:
12 × 4 = 48

Nilai diperoleh:
41

Persentase:
41 / 48 × 100
= 85,42%
```

Sistem kemudian menampilkan hasil observasi.

Contoh:

```text
Hasil Observasi
41 / 48
85,42%
```

Formula final harus mengikuti aturan bisnis yang disepakati, terutama jika nanti menggunakan bobot.

---

# 20. Hasil Observasi

Hasil observasi perlu menyimpan setidaknya:

```text
Guru
Observer
Tanggal observasi
Instrumen
Versi instrumen
Skor setiap indikator
Catatan setiap indikator
Catatan umum
Total skor
Skor maksimum
Persentase
Status
```

Satu guru kemungkinan dapat memiliki **lebih dari satu observasi**.

Contoh:

```text
Guru A

Observasi 1 — September
Observasi 2 — November
Observasi 3 — Januari
```

Jangan mendesain sistem dengan asumsi satu guru hanya bisa memiliki satu hasil observasi.

---

# 21. Refleksi Diri Guru Pasca-Observasi Kelas

Setelah kegiatan observasi kelas dilaksanakan oleh observer (Pengawas/Kepsek), Guru melakukan pengisian **Refleksi Diri Pasca-Observasi**.

Refleksi ini mencakup:
- Kelebihan / hal positif yang dirasakan guru saat mengajar.
- Kendala atau hambatan yang dihadapi di kelas.
- Aspek pembelajaran yang ingin ditingkatkan oleh guru.
- Bentuk dukungan atau pelatihan yang diharapkan dari sekolah/pengawas.

Data refleksi diri guru ini menjadi **salah satu bahan pertimbangan utama** bagi kegiatan tindak lanjut.

---

# 22. AI Setelah Observasi & Refleksi Guru

Setelah observasi dan refleksi guru selesai, AI membantu menganalisis hasil secara menyeluruh.

Input AI berasal dari (Tri-Partit Input):

```text
Hasil perangkat pembelajaran (Fitur 1)
+
Hasil observasi & catatan observer (Fitur 2)
+
Refleksi diri guru pasca-observasi (Fitur 2b)
```

AI dapat menghasilkan:

```text
Ringkasan Sintesis
Area yang sudah baik (Perspektif Observer & Guru)
Area yang perlu perhatian / pengembangan
Rekomendasi Program Tindak Lanjut
```

Contoh konseptual:

```text
Hasil observasi & refleksi guru menunjukkan aspek A
memerlukan perhatian dan dukungan pelatihan.

Rekomendasi:
- memperkuat aspek A,
- memberikan fasilitas pelatihan strategi B,
- melakukan evaluasi berkala.
```

Sekali lagi, hasil tersebut adalah **rekomendasi AI**, bukan keputusan final.

---

# 23. Tindak Lanjut

Tahap pasca-observasi dan refleksi guru adalah penetapan tindak lanjut.

Client mengatakan:

> "untuk tindak lanjut, direkomendasikan oleh AI, setelah observasi, dari AI, berdasarkan dari fitur 1 dan 2 tadi, serta refleksi guru pasca-observasi"

Pemahaman:

AI menggunakan data dari seluruh tahap sebelumnya:

```text
Fitur/hasil 1:
Perangkat Pembelajaran

+

Fitur/hasil 2:
Observasi Pembelajaran

+

Fitur/hasil 2b:
Refleksi Diri Guru Pasca-Observasi

↓

AI

↓

Rekomendasi Tindak Lanjut
```

---

# 23. Rekomendasi Tindak Lanjut AI

Contoh konseptual:

```text
Hasil perangkat:
Asesmen perlu diperbaiki.

Hasil observasi:
Aspek asesmen mendapat skor rendah.

↓

AI Recommendation

1. Penguatan asesmen formatif.
2. Perbaikan rancangan asesmen.
3. Pendampingan terkait strategi asesmen.
```

Rekomendasi tersebut dapat dikroscek lagi oleh Pengawas/Kepala Sekolah.

Client menyebut:

> "bisa dikroscek lagi oleh 2 role itu"

Maka flow:

```text
AI Recommendation
       ↓
Pengawas / Kepala Sekolah
       ↓
Review
       ↓
Edit / Accept / Reject
       ↓
Final Follow-up
```

---

# 24. Tindak Lanjut Bukan Hanya Text

Idealnya tindak lanjut menjadi data yang dapat dimonitor.

Kemungkinan data:

```text
Rekomendasi
Tindakan
Target
Penanggung jawab
Deadline
Status
Catatan
```

Status misalnya:

```text
Draft
Direkomendasikan
Disetujui
Dalam Proses
Selesai
```

Tetapi status final perlu dikonfirmasi.

---

# 25. Laporan Akhir

Output akhir aplikasi adalah laporan supervisi.

Client menyampaikan:

> "hasil akhirnya berupa laporan, mulai dari perencanaan sampai ... baik bisa diunduh"

Artinya laporan sebaiknya mencakup keseluruhan siklus.

Konsep:

```text
Perencanaan
      +
Perangkat Pembelajaran
      +
Verifikasi
      +
Observasi
      +
Hasil Penilaian
      +
Catatan
      +
Tindak Lanjut
      +
Hasil Rekomendasi
      ↓
Laporan Supervisi
```

Laporan dapat diunduh.

Kemungkinan format utama adalah PDF, tetapi format final perlu dikonfirmasi.

---

# 26. Contoh Struktur Laporan

Secara konseptual:

```text
LAPORAN SUPERVISI GURU

Identitas:
- Nama Guru
- Sekolah
- Mata Pelajaran
- Periode

A. PERENCANAAN
- informasi perencanaan

B. PERANGKAT PEMBELAJARAN
- daftar perangkat
- status tersedia
- hasil analisis
- hasil verifikasi

C. OBSERVASI PEMBELAJARAN
- tanggal
- observer
- instrumen
- skor
- persentase
- catatan

D. ANALISIS
- ringkasan hasil
- rekomendasi

E. TINDAK LANJUT
- rekomendasi
- tindakan
- status

F. VERIFIKASI
- Pengawas
- Kepala Sekolah
- pihak terkait
```

Struktur resmi laporan harus dikonfirmasi dengan client.

---

# 27. Perbedaan Pengawas dan Kepala Sekolah

Perbedaan paling penting adalah **scope**.

## Pengawas

```text
Pengawas
│
├── Sekolah A
│   ├── Guru
│   └── Supervisi
│
├── Sekolah B
│   ├── Guru
│   └── Supervisi
│
└── Sekolah C
    ├── Guru
    └── Supervisi
```

Pengawas membutuhkan:
- overview seluruh sekolah,
- perbandingan/rekap antar sekolah,
- detail guru,
- supervisi.

## Kepala Sekolah

```text
Kepala Sekolah
│
└── Sekolah sendiri
    ├── Guru 1
    ├── Guru 2
    ├── Guru 3
    └── ...
```

Kepala Sekolah hanya membutuhkan data sekolahnya.

---

# 28. Prinsip Authorization

Scope data harus dijaga.

Contoh:

```text
Pengawas A
→ hanya sekolah yang ditugaskan ke Pengawas A.

Kepala Sekolah A
→ hanya Sekolah A.

Guru A
→ hanya data miliknya sendiri.
```

Frontend tidak boleh menjadi satu-satunya tempat pembatasan akses.

Authorization harus diperiksa di backend/server.

---

# 29. Peran AI Secara Keseluruhan

AI berada di beberapa titik:

```text
              GOOGLE DRIVE
                   ↓
             DOKUMEN GURU
                   ↓
             ┌───────────┐
             │    AI     │
             │ Perangkat │
             └─────┬─────┘
                   ↓
          Review manusia
                   ↓
             OBSERVASI
                   ↓
             Nilai + Catatan
                   ↓
             ┌───────────┐
             │    AI     │
             │ Observasi │
             └─────┬─────┘
                   ↓
          Review manusia
                   ↓
            TINDAK LANJUT
                   ↓
             ┌───────────┐
             │    AI     │
             │Recommend. │
             └─────┬─────┘
                   ↓
          Review manusia
                   ↓
                LAPORAN
```

AI tidak menggantikan Pengawas/Kepala Sekolah.

---

# 30. Prinsip Human-in-the-Loop

Ini merupakan salah satu requirement paling penting.

AI:

```text
Analyze
Recommend
Summarize
```

Manusia:

```text
Review
Verify
Correct
Approve
Finalize
```

Jangan membuat flow:

```text
AI → langsung Final
```

Flow yang diharapkan:

```text
AI
 ↓
Review
 ↓
Verified
 ↓
Final
```

---

# 31. Konsep Data yang Kemungkinan Dibutuhkan

Ini bukan schema final, tetapi gambaran domain yang muncul dari requirement.

```text
users
roles

schools
school_user_assignments
school_supervisor_assignments

teachers

supervision_periods
supervisions

device_types
documents
teacher_devices
device_assessments

observation_instruments
observation_instrument_versions
observation_indicators

observation_sessions
observation_scores

ai_analyses
ai_recommendations

follow_ups
follow_up_items

reports
audit_logs
```

Nama tabel dapat berubah sesuai stack.

---

# 32. Relationship Konseptual

```text
PENGAWAS
   │
   └── assigned schools
            │
            └── teachers
                    │
                    └── supervision
                           │
                           ├── planning
                           │
                           ├── devices
                           │      │
                           │      └── Google Drive
                           │
                           ├── AI analysis
                           │
                           ├── observation
                           │      ├── instrument
                           │      ├── scores
                           │      └── notes
                           │
                           ├── AI analysis
                           │
                           ├── follow-up
                           │
                           └── report
```

---

# 33. Instrumen Harus Versioned

Jika instrumen observasi pernah digunakan, kemudian diubah, hasil observasi lama tidak boleh berubah secara tidak sengaja.

Contoh:

```text
Instrumen Observasi
│
├── Version 1
│   └── digunakan Observasi September
│
└── Version 2
    └── digunakan Observasi November
```

Observasi harus menyimpan versi instrumen yang digunakan pada saat observasi.

---

# 34. Data AI Harus Terpisah dari Hasil Final

Misalnya AI mengatakan:

```text
confidence: 0.84
finding: ...
recommendation: ...
```

Data tersebut jangan langsung dianggap sebagai hasil final.

Lebih baik:

```text
AI Result
   ↓
Human Review
   ↓
Verified Result
```

Dengan begitu sistem dapat menunjukkan:
- apa yang disarankan AI,
- siapa yang memverifikasi,
- kapan diverifikasi,
- apakah ada perubahan,
- dan apa hasil akhirnya.

---

# 35. Audit Trail

Karena aplikasi berhubungan dengan penilaian/supervisi, perubahan penting sebaiknya dapat dilacak.

Contoh:

```text
User:
Pengawas A

Action:
Verify Observation

Resource:
Observation #123

Time:
2026-09-23 14:32

Result:
Verified
```

Aksi yang penting untuk dipertimbangkan:
- verifikasi perangkat,
- perubahan skor,
- finalisasi observasi,
- persetujuan tindak lanjut,
- finalisasi laporan.

---

# 36. Hal yang Belum Boleh Diasumsikan

Berikut hal-hal yang masih belum jelas dari informasi awal.

## Akun

- Apakah Guru wajib mempunyai akun?
- Siapa yang membuat akun Guru?
- Apakah Kepala Sekolah dibuat oleh Pengawas?
- Apakah ada Admin sistem?

## Sekolah

- Apakah satu sekolah dapat memiliki lebih dari satu Pengawas?
- Apakah satu Pengawas selalu menjadi pemilik/administrator sekolah?
- Apakah Pengawas dapat menghapus sekolah?
- Apakah Kepala Sekolah dapat menambahkan Guru?

## Perangkat

- Daftar perangkat wajib apa saja?
- Apakah perangkat berbeda berdasarkan jenjang?
- Apakah perangkat berbeda berdasarkan mata pelajaran?
- Apa definisi "kualitas perangkat"?
- Bagaimana rubric kualitas ditentukan?

## Google Drive

- Guru memilih folder atau file?
- Apakah Google Drive personal?
- Apakah Shared Drive didukung?
- Apakah Pengawas/Kepala Sekolah membuka file langsung melalui Google Drive?
- Apakah file disalin ke storage aplikasi?
- File apa saja yang wajib didukung?

## AI

- Model AI apa yang digunakan?
- Provider AI apa yang digunakan?
- Apakah semua dokumen boleh dikirim ke provider AI?
- Bagaimana privacy/security dokumen?
- Apakah AI harus memberikan evidence/page reference?
- Bagaimana rubric kualitas perangkat?
- Apakah AI hanya memberikan rekomendasi?

## Observasi

- Apakah instrumen Pengawas dan Kepala Sekolah sama?
- Apakah instrumen dapat berbeda per sekolah?
- Apakah instrumen dapat berbeda per mata pelajaran?
- Apakah skala default 1–3 atau 1–4?
- Apakah setiap indikator memiliki bobot?
- Apa kategori hasil akhirnya?
- Apakah observasi dapat dilakukan berkali-kali?

## Tindak Lanjut

- Siapa yang menyetujui?
- Apakah Guru dapat melihat rekomendasi?
- Apakah ada deadline?
- Apakah ada monitoring?
- Apakah tindak lanjut harus diverifikasi kembali?

## Laporan

- Format resmi laporan?
- PDF saja atau juga Excel?
- Apakah ada kop/logo?
- Apakah perlu tanda tangan?
- Apakah perlu tanda tangan digital?
- Apakah laporan memiliki nomor dokumen?

---

# 37. Hal yang Sebaiknya Tidak Dibuat Hard-coded

Karena requirement masih berkembang, beberapa bagian sebaiknya configurable:

### Jenis perangkat

Jangan:

```text
if file == "modul_ajar"
if file == "rpp"
...
```

Lebih baik:

```text
DeviceType
```

yang dapat dikonfigurasi.

### Instrumen

Jangan membuat hanya:

```text
Indicator 1
...
Indicator 12
```

Lebih baik:

```text
Instrument
  └── Indicators[]
```

Jumlah indikator fleksibel.

### Skala

Jangan mengunci hanya 1–4.

Buat konfigurasi skala.

### Kategori hasil

Jangan langsung mengasumsikan:

```text
90-100 = Sangat Baik
80-89 = Baik
...
```

karena client belum memberikan kategori resmi.

---

# 38. Contoh End-to-End Scenario

Contoh untuk memahami aplikasi:

## Tahap 1 — Pengawas

Pengawas login.

Dia mempunyai:

```text
7 sekolah
```

Pengawas menambahkan/assign sekolah jika belum tersedia.

---

## Tahap 2 — Kepala Sekolah

Kepala Sekolah memiliki:

```text
Sekolah A
15 guru
```

Guru-guru dimasukkan/diundang ke sistem.

---

## Tahap 3 — Guru

Guru login.

Guru menghubungkan Google Drive.

Guru memilih folder:

```text
Supervisi 2026
```

Sistem menemukan:

```text
11 dokumen
```

---

## Tahap 4 — AI

AI membaca dokumen yang tersedia.

AI membantu memetakan:

```text
Dokumen → jenis perangkat
```

Kemudian memberikan analisis.

---

## Tahap 5 — Review

Kepala Sekolah/Pengawas melihat hasil AI.

Mereka melakukan cross-check.

Misalnya:

```text
AI:
Dokumen Asesmen tersedia.

Reviewer:
Tidak sesuai.

Status:
Rejected
```

Hasil final berasal dari reviewer.

---

## Tahap 6 — Observasi

Pengawas/Kepala Sekolah membuat observasi.

Memilih instrumen.

Misalnya:

```text
12 indikator
Skala 1–4
```

Observer mengisi:

```text
Score
Note
```

---

## Tahap 7 — Kalkulasi

Sistem otomatis menghitung:

```text
Total Score
Maximum Score
Percentage
```

---

## Tahap 8 — AI Observasi

AI membaca:

```text
hasil perangkat
+
hasil observasi
+
catatan observer
```

Kemudian membuat rekomendasi.

---

## Tahap 9 — Review Rekomendasi

Pengawas/Kepala Sekolah:

```text
Accept
Edit
Reject
```

---

## Tahap 10 — Tindak Lanjut

Tindakan dibuat berdasarkan hasil final.

Contoh:

```text
Rekomendasi:
Penguatan asesmen formatif.

Target:
Guru memperbaiki perangkat asesmen.

Deadline:
...

Status:
In Progress
```

---

## Tahap 11 — Laporan

Sistem menggabungkan seluruh proses.

```text
Perencanaan
+
Perangkat
+
Verifikasi
+
Observasi
+
Hasil
+
Tindak Lanjut
```

Kemudian menghasilkan laporan yang dapat diunduh.

---

# 39. Prinsip Desain yang Harus Dipertahankan

## 39.1 AI bukan decision maker

AI membantu, manusia memutuskan.

## 39.2 Scope harus ketat

Pengawas hanya sekolah yang menjadi tanggung jawabnya.

Kepala Sekolah hanya sekolahnya.

Guru hanya data miliknya.

## 39.3 History harus dipertahankan

Supervisi adalah proses historis.

Jangan overwrite hasil lama.

## 39.4 Instrumen configurable

Instrumen dapat berubah sesuai kebutuhan.

## 39.5 Hasil AI dapat diverifikasi

Jangan membuat hasil AI immutable/final.

## 39.6 Dokumen harus traceable

Jika AI menyatakan sesuatu mengenai dokumen, idealnya sistem dapat menunjukkan dokumen sumbernya.

## 39.7 Workflow manual harus tetap berjalan

Jika AI gagal, supervisi manual tetap dapat dilakukan.

---

# 40. Prinsip untuk AI Coding Agent

AI coding agent yang mengerjakan repository ini harus memahami:

### Jangan langsung coding dari asumsi.

Pertama:
1. baca repository,
2. baca dokumen ini,
3. identifikasi stack,
4. pecah requirement,
5. identifikasi requirement yang belum jelas,
6. baru implementasi.

### Jangan mengarang business rule.

Jika informasi tidak ada:

```text
DECISION REQUIRED
```

atau gunakan desain configurable.

### Jangan menganggap angka contoh sebagai aturan.

Contoh:
- 7 sekolah,
- 15 guru,
- 11 perangkat,
- 12 indikator,
- skala 1–3/1–4,
- 85,42%

adalah contoh untuk menjelaskan konsep.

Bukan nilai tetap.

### Jangan menganggap AI output sebagai final.

Selalu pisahkan:

```text
AI Generated
```

dari:

```text
Human Verified
```

### Jangan melakukan rewrite besar tanpa alasan.

Ikuti architecture dan convention repository yang sudah ada.

---

# 41. Prioritas Implementasi yang Dipahami

Jika aplikasi harus dibangun bertahap, urutan konseptual yang masuk akal:

```text
1. Authentication & Roles
        ↓
2. School Management
        ↓
3. Teacher Management
        ↓
4. Learning Device Management
        ↓
5. Google Drive Integration
        ↓
6. Device Verification
        ↓
7. Observation Instrument
        ↓
8. Observation Session
        ↓
9. Scoring & Result
        ↓
10. AI Analysis
        ↓
11. Follow-up
        ↓
12. AI Recommendation
        ↓
13. Dashboard
        ↓
14. Reporting
```

AI coding agent boleh memecah lagi menjadi task-task kecil.

---

# 42. Gambaran Akhir Produk

Pada akhirnya aplikasi yang diinginkan dapat dipahami sebagai:

```text
                    SISTEM SUPERVISI GURU
                              │
          ┌───────────────────┼───────────────────┐
          │                   │                   │
      PENGAWAS         KEPALA SEKOLAH          GURU
          │                   │                   │
    Banyak sekolah       Satu sekolah       Data sendiri
          │                   │                   │
          └───────────────┬───┴───────────────────┘
                          │
                  SUPERVISI GURU
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
   PERENCANAAN       OBSERVASI         TINDAK LANJUT
        │                 │                 │
   Google Drive       Instrumen          AI
        │              Scoring         Recommendation
        │              Catatan              │
        └───────────────┼───────────────────┘
                        │
                       AI
                        │
                Analisis & Rekomendasi
                        │
                  Human Verification
                        │
                      LAPORAN
```

---

# 43. Kesimpulan Pemahaman Requirement

Secara keseluruhan, aplikasi ini bukan sekadar aplikasi upload dokumen.

Aplikasi merupakan **platform workflow supervisi guru** yang menghubungkan:

```text
Pengawas
   ↓
Sekolah
   ↓
Kepala Sekolah
   ↓
Guru
   ↓
Perangkat Pembelajaran
   ↓
Google Drive
   ↓
AI Analysis
   ↓
Human Verification
   ↓
Observasi Pembelajaran
   ↓
Scoring
   ↓
AI Analysis
   ↓
Tindak Lanjut
   ↓
AI Recommendation
   ↓
Human Verification
   ↓
Laporan
```

Ada tiga fungsi besar:

### 1. Document/Planning Management

Mengelola dan memeriksa perangkat pembelajaran yang disediakan guru melalui Google Drive.

### 2. Supervision/Observation Management

Mengelola instrumen observasi, penilaian, catatan, kalkulasi hasil, dan histori observasi.

### 3. AI-Assisted Follow-up & Reporting

AI membantu menganalisis data dan membuat rekomendasi, kemudian hasilnya diverifikasi manusia dan dimasukkan ke laporan.

---

# 44. Instruksi Terakhir untuk Agent

**Jadikan dokumen ini sebagai konteks utama, bukan sebagai alasan untuk mengarang requirement.**

Agent diharapkan memecah dokumen ini menjadi dokumentasi teknis/produk yang sesuai dengan repository.

Contoh pemecahan:

```text
docs/
├── product-requirements.md
├── roles-permissions.md
├── business-workflow.md
├── domain-model.md
├── database-schema.md
├── google-drive-integration.md
├── ai-architecture.md
├── observation-system.md
├── follow-up-system.md
├── dashboard.md
├── reporting.md
├── api.md
├── security.md
├── testing.md
└── implementation-plan.md
```

Namun pemecahan tersebut adalah tugas agent setelah memahami keseluruhan konteks.

**Jangan membuat implementasi berdasarkan bagian yang masih ditandai belum jelas sebagai seolah-olah sudah diputuskan.**
