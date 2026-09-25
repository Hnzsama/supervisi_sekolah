# 09 — Dashboard & Reporting

## 1. Dashboard Supervisi Multi-Role

Dashboard menyajikan statistik dan perkembangan proses supervisi disesuaikan dengan hak akses dan cakupan role pengguna.

### 1.1 Dashboard Pengawas Sekolah (Overview Multi-Sekolah)
Menampilkan statistik agregat dari **seluruh sekolah dampingan** yang berada di bawah pengawasannya.

```mermaid
graph TD
    subgraph SP["SUMMARY PENGAWAS: 7 Sekolah | 105 Guru"]
        M1["Kelengkapan Perangkat: 82%"]
        M2["Observasi: 74%"]
        M3["Tindak Lanjut: 61%"]
    end

    subgraph DSL["DAFTAR SEKOLAH DAMPINGAN"]
        S1["1. SMA Negeri 1 (15 Guru) - Perangkat 87% | Observasi 80%"]
        S2["2. SMA Negeri 2 (20 Guru) - Perangkat 79% | Observasi 70%"]
        S3["3. SMA Negeri 3 (18 Guru) - Perangkat 91% | Observasi 85%"]
    end

    SP --> DSL
```

- **Fitur Tambahan:**
  - Tombol *Add Sekolah Manual* untuk menambah sekolah baru.
  - Filter berdasarkan periode supervisi / tahun ajaran.

### 1.2 Dashboard Kepala Sekolah (Overview Single-Sekolah)
Menampilkan perkembangan supervisi dari **seluruh guru di sekolahnya saja**.

```mermaid
graph TD
    subgraph KS["SUMMARY KEPALA SEKOLAH: SMA Negeri 1 (15 Guru)"]
        KM1["Kelengkapan Perangkat: 87%"]
        KM2["Observasi: 73%"]
        KM3["Tindak Lanjut: 55%"]
    end

    subgraph DGL["DAFTAR GURU SEKOLAH"]
        G1["1. Ahmad Dahlan - Matematika (Score: 85,42%)"]
        G2["2. Siti Walidah - Bahasa Indo (Score: 78,00%)"]
    end

    KS --> DGL
```

---

## 2. Spesifikasi Laporan Akhir Supervisi

Output akhir dari seluruh rangkaian siklus supervisi adalah **Laporan Supervisi Akademik Guru**.

### 2.1 Struktur Konten Laporan Supervisi
Laporan supervisi merangkum seluruh perjalanan dari perencanaan hingga tindak lanjut:

```mermaid
flowchart TD
    I["I. IDENTITAS GURU & SEKOLAH<br/>Nama Guru, NIP, Mapel, Sekolah, Observer"] --> II["II. PERENCANAAN & PERANGKAT PEMBELAJARAN<br/>Tabel Status Berkas & Catatan Verifikasi"]
    II --> III["III. OBSERVASI PELAKSANAAN PEMBELAJARAN<br/>Instrumen, Skor, Persentase (85,42%), Catatan"]
    III --> IV["IV. LEMBAR REFLEKSI DIRI GURU<br/>Hal Baik, Kendala, Area Perbaikan & Support Needed"]
    IV --> V["V. SYNTHESIS ANALISIS & REKOMENDASI AI<br/>Sintesis Observasi & Refleksi Guru"]
    V --> VI["VI. RENCANA TINDAK LANJUT SUPERVISI<br/>Action Plan, Target Waktu, Status"]
    VI --> VII["VII. LEMBAR PENGESAHAN & VERIFIKASI<br/>Tanda Tangan Guru, Kepsek, Pengawas"]
```

### 2.2 Format Output & Unduh
- **Dukungan Format:** Draf laporan dapat dilihat secara langsung di web (*Interactive View*) dan diunduh sebagai dokumen berkas **PDF**.
- **Fitur Kop & Identitas:** Dokumen mendukung penggunaan kop surat resmi sekolah/dinas dan kolom tanda tangan pengesahan.
