# 11 — Open Questions & Decision Log

> **Dokumen ini mencatat seluruh poin yang belum diputuskan secara final, asumsi yang digunakan sementara, dan hal-hal yang membutuhkan konfirmasi lebih lanjut dari client/product owner.**

---

## 1. Daftar Pertanyaan Terbuka per Domain

### 1.1 Akun & Otorisasi Pengguna
| ID | Topik | Pertanyaan Terbuka | Asumsi Sementara yang Digunakan | Status |
| :--- | :--- | :--- | :--- | :--- |
| `Q-ACC-01` | Akun Guru | Apakah Guru wajib membuat akun mandiri atau dibuatkan oleh Kepsek/Pengawas? | Akun Guru diundang/didaftarkan oleh Kepsek/Admin Sekolah. | `OPEN` |
| `Q-ACC-02` | Admin Sistem | Apakah ada role Super Admin yang mengelola seluruh sistem? | Ada role Admin Sistem untuk manajemen master awal. | `OPEN` |
| `Q-ACC-03` | Penambahan Sekolah | Saat Pengawas menambah sekolah manual, apakah sekaligus membuatkan akun Kepsek? | Pengawas hanya memasukkan master data sekolah, akun Kepsek dibuat terpisah. | `OPEN` |

### 1.2 Integrasi Google Drive & Dokumen
| ID | Topik | Pertanyaan Terbuka | Asumsi Sementara yang Digunakan | Status |
| :--- | :--- | :--- | :--- | :--- |
| `Q-GDR-01` | Shared Drive | Apakah sistem mendukung Google Shared Drive milik sekolah? | Mendukung *My Drive* dan *Shared Drive*. | `OPEN` |
| `Q-GDR-02` | Local Copy | Apakah file dari GDrive disalin ke storage lokal aplikasi? | File tidak disalin, hanya membaca metadata & view via URL GDrive. | `OPEN` |
| `Q-GDR-03` | Jenis File | Ekstensi file apa saja yang diproses AI? | PDF, DOCX, dan Google Docs. | `OPEN` |

### 1.3 AI Engine & Privacy
| ID | Topik | Pertanyaan Terbuka | Asumsi Sementara yang Digunakan | Status |
| :--- | :--- | :--- | :--- | :--- |
| `Q-AI-01` | Provider AI | Provider/Model AI apa yang digunakan (OpenAI, Gemini, Local LLM)? | Arsitektur disiapkan terisolasi (*Provider Agnostic*). | `OPEN` |
| `Q-AI-02` | Privacy Data | Apakah seluruh isi dokumen guru diizinkan dikirim ke Cloud AI? | Dilakukan filtering PII sebelum dikirim ke API AI. | `OPEN` |

### 1.4 Observasi & Instrumen
| ID | Topik | Pertanyaan Terbuka | Asumsi Sementara yang Digunakan | Status |
| :--- | :--- | :--- | :--- | :--- |
| `Q-OBS-01` | Pembelajaran Mendalam | Apakah indikator *Pembelajaran Mendalam* memiliki standar resmi dinas? | Disediakan template dasar yang dapat disunting (configurable). | `OPEN` |
| `Q-OBS-02` | Bobot Indikator | Apakah setiap indikator memilik bobot yang berbeda dalam skor akhir? | Semua indikator memiliki bobot sama (skala 1–4 sederhana). | `OPEN` |
| `Q-OBS-03` | Predikat Nilai | Bagaimana rentang persentase untuk predikat (Sangat Baik, Baik, Cukup)? | Dikonfigurasi dalam tabel threshold (misal >85% Sangat Baik). | `OPEN` |

### 1.5 Tindak Lanjut & Laporan
| ID | Topik | Pertanyaan Terbuka | Asumsi Sementara yang Digunakan | Status |
| :--- | :--- | :--- | :--- | :--- |
| `Q-REP-01` | Format Laporan | Apakah format laporan harus 100% persis dengan template dinas tertentu? | Mengikuti struktur standar 6 bagian laporan supervisi. | `OPEN` |
| `Q-REP-02` | Digital Signature | Apakah laporan memerlukan tanda tangan digital (QR Code / E-Meterai)? | Menyiapkan area tanda tangan visual/konvensional. | `OPEN` |

---

## 2. Catatan Keputusan (Decision Required Log)

Setiap kali ada keputusan baru dari client/pengguna, catat pada bagian ini:

```text
[Tanggal] — [ID Pertanyaan] — [Keputusan Final] — [Disetujui Oleh]
------------------------------------------------------------------
Contoh:
2026-09-23 — Q-OBS-02 — Diputuskan bobot indikator sama rata — Client
```
