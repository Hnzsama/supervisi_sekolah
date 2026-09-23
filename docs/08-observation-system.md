# 08 — Observation System & Scoring

## 1. Konsep Sistem Observasi Pembelajaran

Observasi pembelajaran dilaksanakan ketika Pengawas atau Kepala Sekolah melakukan pengamatan langsung kegiatan belajar mengajar di kelas.

Sistem observasi menyediakan antarmuka pengisian instrumen berbasis web/mobile-friendly yang dinamis dan fleksibel.

```mermaid
flowchart TD
    subgraph INSTRUMENT["Instrumen Observasi"]
        IND["Indikator Penilaian<br/>(Configurable)"]
        SCL["Skala Penilaian<br/>(1-3 atau 1-4)"]
    end

    subgraph FORM["Form Observasi Real-time"]
        SCORE["Pilih Nilai per Indikator"]
        NOTE["Catatan Lapangan Observer"]
    end

    subgraph CALC["Kalkulasi Skor Otomatis"]
        TOT["Total Perolehan / Maksimal"]
        PCT["Persentase Ketercapaian (%)"]
    end

    IND --> SCORE
    SCL --> SCORE
    SCORE --> TOT
    NOTE --> TOT
    TOT --> PCT
```

---

## 2. Manajemen Instrumen Observasi Fleksibel

Berdasarkan kebutuhan pengguna:

> Pengawas dan Kepala Sekolah diberikan kebebasan untuk memasukkan instrumen penilaian sendiri yang akan menjadi penilai kunjungan di dalam kelas.

### Ketentuan Instrumen:
1. **Configurable Indicators:** Jumlah indikator bersifat dinamis (tidak di-hardcode 12 indikator).
2. **Kategori / Domain Penilaian:** Indikator dapat dikelompokkan ke dalam kategori tertentu, misalnya **Pendekatan Pembelajaran Mendalam (Deep Learning Approach)**, Manajemen Kelas, Interaksi Siswa, dll.
3. **Versi Instrumen (Versioning):** Perubahan indikator akan menghasilkan versi instrumen baru agar histori observasi sebelumnya tidak rusak.

---

## 3. Skala Penilaian & Catatan per Indikator

### 3.1 Skala Penilaian
Sistem mendukung konfigurasi rentang skala penilaian:
- **Skala 1–4:** `1 = Kurang`, `2 = Cukup`, `3 = Baik`, `4 = Sangat Baik`
- **Skala 1–3:** `1 = Belum Nampak`, `2 = Mulai Nampak`, `3 = Sudah Pembiasaan`

### 3.2 Catatan Kualitatif per Indikator
Setiap item indikator penilaian dilengkapi dengan kolom **Catatan Observer**.
- Contoh:
  ```text
  Indikator: Guru melibatkan peserta didik secara aktif dalam diskusi kelompok.
  Skor     : 4 (Sangat Baik)
  Catatan  : Peserta didik sangat antusias, diskusi kelompok berjalan efektif.
  ```

---

## 4. Formulasi Akumulasi Skor & Presentasi Hasil

### 4.1 Rumus Perhitungan Nilai Akhir Observasi

$$\text{Nilai Akhir (\%)} = \left( \frac{\sum \text{Skor Perolehan Indikator}}{\sum \text{Skor Maksimal Indikator}} \right) \times 100\%$$

*Contoh Kasus (12 Indikator, Skala Maksimal 4):*
- Total Skor Maksimal = $12 \times 4 = 48$
- Total Skor Perolehan = $41$
- Persentase Akhir = $\left(\frac{41}{48}\right) \times 100\% = 85{,}42\%$

### 4.2 Penyimpanan Data Observasi
Satu guru dapat memiliki **banyak riwayat observasi** dalam satu tahun ajaran (misal: Observasi Awal, Observasi Lanjutan, Observasi Akhir). Setiap riwayat wajib mencatat:
- Tanggal observasi & Jam pelaksanaan.
- Observer (Pengawas atau Kepala Sekolah).
- Versi instrumen yang digunakan.
- Detail skor dan catatan per indikator.
- Total skor, nilai maksimal, dan persentase akhir.
- Catatan umum observer.
