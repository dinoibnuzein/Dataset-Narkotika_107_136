# Dataset-Narkotika_XXX_YYY

> Kumpulan putusan Pengadilan Negeri Rantau Prapat perkara **Narkotika dan Psikotropika** (Pidana Khusus), tahun 2023–2025, untuk riset dan praktikum **Temu Kembali Informasi (Information Retrieval)**.

![Format](https://img.shields.io/badge/format-PDF%20%2B%20XLSX-blue)
![Domain](https://img.shields.io/badge/domain-hukum-8a2be2)
![Bahasa](https://img.shields.io/badge/bahasa-Indonesia-red)
![Status](https://img.shields.io/badge/status-bukan%20inkracht-orange)

Ganti `XXX` dan `YYY` pada judul dengan 3 digit terakhir NIM anggota 1 dan anggota 2.

---

## Daftar Isi

1. [Tentang Dataset](#tentang-dataset)
2. [Sumber Data](#sumber-data)
3. [Kriteria Pengumpulan](#kriteria-pengumpulan)
4. [Struktur Repository](#struktur-repository)
5. [Isi Overview.xlsx](#isi-overviewxlsx)
6. [Penamaan File PDF](#penamaan-file-pdf)
7. [Cara Memakai](#cara-memakai)
8. [Contoh Kegunaan](#contoh-kegunaan)
9. [Anatomi Putusan Narkotika](#anatomi-putusan-narkotika)
10. [Keterbatasan](#keterbatasan)
11. [Etika dan Legalitas](#etika-dan-legalitas)
12. [Tim](#tim)

---

## Tentang Dataset

Putusan pengadilan adalah teks hukum yang panjang, formal, dan punya pola kalimat yang berulang. Sifat ini membuatnya cocok sebagai korpus untuk Temu Kembali Informasi: ada struktur yang bisa diekstraksi, kosakata yang khas, dan kebutuhan pencarian yang nyata (misalnya mencari putusan dengan barang bukti tertentu atau pidana tertentu).

Dataset ini berisi **50 putusan** dalam bentuk PDF, ditambah satu berkas ringkasan `Overview.xlsx` yang memuat metadata dan dua bagian inti tiap putusan: **barang bukti** dan **amar putusan**.

| Item | Keterangan |
|---|---|
| Jumlah dokumen | 50 putusan |
| Format dokumen | PDF |
| Format ringkasan | XLSX |
| Pengadilan | PN Rantau Prapat |
| Kategori | Pidana Khusus, Narkotika dan Psikotropika |
| Rentang tahun | 2023–2025 |
| Bahasa | Indonesia |

## Sumber Data

Seluruh putusan diunduh dari **Direktori Putusan Mahkamah Agung Republik Indonesia**:

- Portal: https://putusan3.mahkamahagung.go.id/direktori.html
- Jalur penelusuran: Direktori → Pidana Khusus → Narkotika dan Psikotropika → PN Rantau Prapat → filter tahun

Pengunduhan dilakukan melalui antarmuka web resmi dengan jeda antar permintaan agar tidak membebani server.

## Kriteria Pengumpulan

Dokumen dipilih dengan aturan berikut:

1. Berasal dari direktori **Pidana Khusus** dengan klasifikasi **Narkotika dan Psikotropika**.
2. Berasal dari **satu pengadilan negeri saja**, yaitu PN Rantau Prapat.
3. Berasal dari tahun putusan **2023, 2024, dan 2025**.
4. **Tidak** berstatus berkekuatan hukum tetap (inkracht).
5. Berekstensi `.pdf` dan dapat dibaca (teks dapat diekstraksi).

## Struktur Repository

```
Dataset-Narkotika_XXX_YYY/
├── Dataset/
│   └── Narkotika.zip        # 50 berkas putusan (*.pdf)
├── Overview/
│   └── Overview.xlsx        # ringkasan 50 putusan
└── README.md
```

## Isi Overview.xlsx

Satu baris mewakili satu putusan. Kolomnya:

| Kolom | Isi |
|---|---|
| `No` | Nomor urut |
| `No Putusan` | Nomor perkara, misalnya `363/Pid.Sus/2024/PN Rap` |
| `Lembaga Peradilan` | Nama pengadilan, misalnya `PN Rantau Prapat` |
| `Barang Bukti` | Daftar barang bukti yang ditetapkan dalam putusan |
| `Amar Putusan` | Bunyi amar putusan: pernyataan bersalah, pidana penjara, denda, status barang bukti, dan biaya perkara |

Contoh baris:

| No | No Putusan | Lembaga Peradilan | Barang Bukti | Amar Putusan |
|---|---|---|---|---|
| 1 | 408/Pid.Sus/2021/PN Gpr | PN Kab Kediri | Narkotika jenis sabu-sabu dalam 4 plastik klip, 1 timbangan digital, 1 HP | Menyatakan Terdakwa terbukti bersalah ... pidana penjara 7 tahun ... denda Rp 1.000.000.000 ... |

Baris contoh di atas hanya ilustrasi format, bukan bagian dari dataset ini.

## Penamaan File PDF

Setiap PDF memakai pola:

```
putusan_<nomor>_<jenis>_<tahun>_<pn>_<yyyymmddHHMMSS>.pdf
```

Contoh: `putusan_363_pid.sus_2024_pn_rap_20261104153012.pdf`

| Bagian | Arti |
|---|---|
| `nomor` | Nomor urut perkara |
| `jenis` | Jenis perkara, mis. `pid.sus` |
| `tahun` | Tahun perkara |
| `pn` | Kode pengadilan, mis. `pn_rap` |
| `yyyymmddHHMMSS` | Waktu pengunduhan |

## Cara Memakai

Unduh dan ekstrak:

```bash
git clone https://github.com/<username>/Dataset-Narkotika_XXX_YYY.git
cd Dataset-Narkotika_XXX_YYY
unzip Dataset/Narkotika.zip -d Dataset/pdf
```

Baca ringkasan dengan Python:

```python
import pandas as pd

df = pd.read_excel("Overview/Overview.xlsx")
print(df.shape)
print(df.head())
```

Baca teks dari PDF:

```python
from pypdf import PdfReader

reader = PdfReader("Dataset/pdf/<nama_file>.pdf")
teks = "\n".join(page.extract_text() or "" for page in reader.pages)
print(teks[:1000])
```

## Contoh Kegunaan

- **Mesin pencari putusan.** Bangun indeks (TF-IDF, BM25) dan cari putusan berdasarkan kata kunci seperti `sabu`, `ganja`, atau `Pasal 114`.
- **Ekstraksi informasi.** Ambil barang bukti, lama pidana, dan besar denda dengan regex atau model NER.
- **Klasifikasi.** Kelompokkan putusan menurut jenis narkotika atau rentang pidana.
- **Evaluasi retrieval.** Susun query dan kunci relevansi sendiri untuk menghitung precision, recall, dan MAP.
- **Analisis teks hukum.** Teliti pola kalimat, istilah, dan struktur dokumen putusan.

## Anatomi Putusan Narkotika

Putusan pidana narkotika punya kerangka yang cukup konsisten:

1. **Kepala putusan:** nomor perkara, nama pengadilan, identitas terdakwa.
2. **Dakwaan dan tuntutan:** pasal yang didakwakan dan tuntutan penuntut umum.
3. **Pertimbangan hukum:** fakta persidangan, alat bukti, unsur pasal.
4. **Amar putusan:** diawali kata `MENGADILI`; memuat vonis, pidana, denda, dan status barang bukti.
5. **Penutup:** tanggal putusan, majelis hakim, panitera.

Kerangka ini membantu ekstraksi: barang bukti dan amar putusan dapat dikenali lewat penanda `barang bukti berupa` dan `MENGADILI`.

## Keterbatasan

- Hanya satu pengadilan negeri, sehingga gaya penulisan dan sebaran kasus belum mewakili seluruh Indonesia.
- Hanya putusan yang belum berkekuatan hukum tetap; status dapat berubah setelah pengunduhan.
- Kualitas teks bergantung pada PDF. Beberapa dokumen berasal dari hasil pindai dan dapat menghasilkan teks yang tidak rapi.
- Setiap halaman PDF memuat penafian dari portal Mahkamah Agung yang perlu dibersihkan saat praproses.
- Ukuran 50 dokumen cocok untuk latihan, tetapi terlalu kecil untuk melatih model skala besar.

## Etika dan Legalitas

Putusan pengadilan adalah informasi publik yang dipublikasikan oleh Mahkamah Agung RI melalui portal resminya. Dataset ini dikumpulkan secara manual dari portal tersebut untuk tujuan pendidikan dan penelitian.

Mohon gunakan dengan bijak. Dokumen memuat nama pihak yang berperkara, jadi jangan dipakai untuk menyebarkan data pribadi atau menstigma pihak tertentu. Hak atas isi putusan tetap pada pihak yang berwenang; repositori ini tidak mengklaim kepemilikan atas dokumen asli.

## Tim

| Nama | NIM | Peran |
|---|---|---|
| `<Nama Anggota 1>` | `<NIM>` | Pengumpulan dan penyusunan dataset |
| `<Nama Anggota 2>` | `<NIM>` | Pembuatan ringkasan dan dokumentasi |

Mata kuliah Temu Kembali Informasi, Semester Ganjil 2026/2027, Fakultas Teknik, Universitas Muhammadiyah Malang.
