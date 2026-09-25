# 11 — Open Questions & Decision Log

> **Dokumen ini mencatat seluruh poin yang belum diputuskan secara final, asumsi yang digunakan sementara, dan hal-hal yang membutuhkan konfirmasi lebih lanjut dari client/product owner.**

---

## 1. Daftar Pertanyaan Terbuka per Domain

### 1.1 Akun & Otorisasi Pengguna
| ID | Topik | Pertanyaan Terbuka | Keputusan Final | Status |
| :--- | :--- | :--- | :--- | :--- |
| `Q-ACC-01` | Alur Pendaftaran | Apakah registrasi bersifat terbuka atau via email invitation? | User diundang via email invitation dengan token. Pengurus wajib membuat data Sekolah lebih dulu, lalu invite Kepsek. Kepsek/Pengurus lalu invite Guru. | `RESOLVED` |
| `Q-ACC-02` | Admin Sistem | Apakah ada role Super Admin yang mengelola seluruh sistem? | Ada role Admin Sistem / Pengurus untuk manajemen master awal dan sekolah. | `OPEN` |
| `Q-ACC-03` | Pembuatan Sekolah | Bagaimana hubungan penambahan sekolah dan akun Kepsek? | Pengurus wajib membuat data sekolah terlebih dahulu, baru kemudian mengundang Kepala Sekolah ke sekolah tersebut via email invitation. | `RESOLVED` |

### 1.2 Integrasi Google Drive & Dokumen
| ID | Topik | Pertanyaan Terbuka | Keputusan Final | Status |
| :--- | :--- | :--- | :--- | :--- |
| `Q-GDR-01` | Shared Drive | Apakah sistem mendukung Google Shared Drive milik sekolah? | Mendukung *My Drive* dan *Shared Drive* via public link. | `OPEN` |
| `Q-GDR-02` | Local Copy | Apakah file dari GDrive disalin ke storage lokal aplikasi? | File GDrive tidak disalin, tetapi file direct upload disalin ke storage internal. | `OPEN` |
| `Q-GDR-03` | Format File Upload | Ekstensi file apa saja yang diizinkan untuk diunggah oleh Guru? | Guru bisa mengunggah berkas dalam **format apapun** (DOCX, PDF, XLSX/Excel, PPTX/PPT, Gambar/Scan, TXT) maupun Google Drive Public Link. | `RESOLVED` |

### 1.3 AI Engine & Privacy
| ID | Topik | Pertanyaan Terbuka | Asumsi Sementara yang Digunakan | Status |
| :--- | :--- | :--- | :--- | :--- |
| `Q-AI-01` | Provider AI | Provider/Model AI apa yang digunakan (OpenAI, Gemini, Local LLM)? | Arsitektur disiapkan terisolasi (*Provider Agnostic*). | `OPEN` |
| `Q-AI-02` | Privacy Data | Apakah seluruh isi dokumen guru diizinkan dikirim ke Cloud AI? | Dilakukan filtering PII sebelum dikirim ke API AI. | `OPEN` |

### 1.4 Observasi & Instrumen
| ID | Topik | Pertanyaan Terbuka | Asumsi Sementara yang Digunakan / Keputusan | Status |
| :--- | :--- | :--- | :--- | :--- |
| `Q-OBS-01` | Pembelajaran Mendalam | Apakah indikator *Pembelajaran Mendalam* memiliki standar resmi dinas? | Disediakan template dasar yang dapat disunting (configurable). | `OPEN` |
| `Q-OBS-02` | Bobot Indikator | Apakah setiap indikator memilik bobot yang berbeda dalam skor akhir? | Semua indikator memiliki bobot sama (skala 1–4 sederhana). | `OPEN` |
| `Q-OBS-03` | Predikat Nilai | Bagaimana rentang persentase untuk predikat (Sangat Baik, Baik, Cukup)? | Dikonfigurasi dalam tabel threshold (misal >85% Sangat Baik). | `OPEN` |
| `Q-OBS-04` | Refleksi Guru | Apakah ada tahap refleksi guru pasca-observasi kelas? | Wajib ditambahkan tahap Refleksi Guru setelah observasi kelas, dan dijadikan salah satu bahan pertimbangan utama dalam kegiatan tindak lanjut. | `RESOLVED` |

### 1.5 Tindak Lanjut & Laporan
| ID | Topik | Pertanyaan Terbuka | Asumsi Sementara yang Digunakan | Status |
| :--- | :--- | :--- | :--- | :--- |
| `Q-REP-01` | Format Laporan | Apakah format laporan harus 100% persis dengan template dinas tertentu? | Mengikuti struktur standar 7 bagian laporan supervisi (termasuk lembar refleksi guru). | `OPEN` |
| `Q-REP-02` | Digital Signature | Apakah laporan memerlukan tanda tangan digital (QR Code / E-Meterai)? | Menyiapkan area tanda tangan visual/konvensional. | `OPEN` |

---

## 2. Catatan Keputusan (Decision Required Log)

Setiap kali ada keputusan baru dari client/pengguna, catat pada bagian ini:

```text
[Tanggal] — [ID Pertanyaan] — [Keputusan Final] — [Disetujui Oleh]
------------------------------------------------------------------
2026-09-24 — Q-ACC-01 — User Onboarding dilakukan via Email Invitation. Pengurus wajib membuat data Sekolah terlebih dahulu sebagai wadah, lalu mengundang Kepsek via email invitation. Kepsek/Pengurus mengundang Guru via email. — User / Client Requirement
2026-09-24 — Q-ACC-03 — Pembuatan Sekolah adalah PRASYARAT MUTLAK sebelum pendaftaran Kepsek dan Guru. — User / Client Requirement
2026-09-24 — Q-GDR-03 — Guru dapat mengunggah berkas dalam FORMAT APA PUN (DOCX, PDF, XLSX, PPTX, Scan/Gambar, dll.) maupun Public Link GDrive. — User / Client Requirement
2026-09-26 — Q-OBS-04 — Ditambahkan TAHAP REFLEKSI GURU setelah kegiatan observasi kelas. Data refleksi diri ini menjadi salah satu bahan pertimbangan utama untuk menyusun rekomendasi dan kegiatan tindak lanjut supervisi. — User / Client Requirement
```

