# 06 — Google Drive Integration (Public Link Model)

## 1. Konsep Penautan Google Drive via Public Link

> **Koreksi Konteks:** Aplikasi **TIDAK menggunakan OAuth2 API Google Drive** kompleks.
> Guru **tidak perlu mengintegrasikan akun Google secara OAuth**.
> Sebagai gantinya, Guru menyimpan dokumen perangkat pembelajaran di Google Drive masing-masing, mengatur hak akses berkas/folder menjadi **"Anyone with the link can view" (Siapa saja yang memiliki link dapat melihat)**, lalu menempelkan (paste) tautan/link publik tersebut ke aplikasi.

```mermaid
flowchart TD
    A[Guru menyusun dokumen di Google Drive] --> B[Set akses berkas/folder:<br/>'Anyone with the link can view']
    B --> C[Guru paste Public Link<br/>Folder / File ke Aplikasi]
    C --> D[Aplikasi & AI membaca berkas<br/>via Tautan Publik]
    D --> E[Pengawas / Kepsek Preview Dokumen<br/>& AI Analisis Kelengkapan]
```

---

## 2. Alur Penggunaan & Penyediaan Tautan

```mermaid
sequenceDiagram
    autonumber
    actor G as Guru
    participant APP as Aplikasi Supervisi
    participant AI as Modul AI
    actor R as Reviewer (Pengawas/Kepsek)

    G->>G: Buka Google Drive & Copy Link "Anyone with link"
    G->>APP: Paste URL Public Drive (Folder/File)
    APP->>APP: Validasi Format URL & Cek Aksesibilitas
    AI->>APP: Extrak Isi Dokumen Publik & Analisis Komponen
    APP->>R: Tampilkan Embedded Preview & Hasil Rekomendasi AI
```

---

## 3. Keunggulan & Batasan Model Public Link

### Keunggulan:
- **Tanpa OAuth Complex:** Tidak membutuhkan registrasi Google App Console, verifikasi OAuth, atau manajemen refresh token yang rumit.
- **Sederhana bagi Guru:** Guru hanya perlu mengopi tautan berbagi yang sudah biasa mereka gunakan.
- **Privasi Terkontrol:** Guru memegang kendali penuh atas folder/berkas apa yang dibagikan melalui link.

### Ketentuan & Batasan:
- **Pengaturan Izin Akses:** Guru wajib memastikan pengaturan berbagi berkas/folder di Google Drive sudah benar (*Anyone with the link can view*). Jika link bersifat privat, sistem/AI dan Pengawas tidak akan bisa mengakses dokumen tersebut.
- **Deteksi Link Privat:** Aplikasi perlu memberikan peringatan jika tautan yang dimasukkan tidak dapat diakses secara publik.
