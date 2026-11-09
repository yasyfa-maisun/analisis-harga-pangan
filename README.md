# Analisis Perubahan Harga Pangan di Pulau Jawa (Desember 2023 – September 2026)

Analisis data harga enam komoditas pangan strategis di enam provinsi Pulau Jawa, dibandingkan dengan inflasi bulanan. Proyek ini mencari tahu komoditas dan provinsi mana yang paling terdampak lonjakan harga 2026, apakah harga September 2026 tergolong tinggi menurut kebiasaan tiap provinsi sendiri, dan apakah perubahan harga pangan berkaitan dengan inflasi.

**Penulis:** Yasyfa Maisun Majidah

**Tools:** Python (pandas, NumPy, SciPy, Matplotlib), Google Colab

---

## Pertanyaan yang dijawab

1. Komoditas dan provinsi mana yang paling terdampak lonjakan harga pangan pada 2026?
2. Apakah harga September 2026 tinggi dibanding kebiasaan harga tiap provinsi sendiri, bukan hanya dibanding setahun lalu?
3. Apakah perubahan harga pangan berkaitan dengan inflasi bulanan?

## Cakupan dan data

| | |
|---|---|
| Komoditas | Beras medium I, daging ayam, telur ayam, bawang merah, cabai merah keriting, cabai rawit merah |
| Provinsi | Banten, DKI Jakarta, Jawa Barat, Jawa Tengah, DI Yogyakarta, Jawa Timur |
| Periode harga | 1 Desember 2023 – 30 September 2026 (harian, hari kerja) |
| Periode inflasi | Januari 2024 – Agustus 2026 (bulanan) |
| Sumber harga | [PIHPS Bank Indonesia](https://www.bi.go.id/hargapangan) |
| Sumber inflasi | [BPS](https://www.bps.go.id) (inflasi bulanan per kota) |

Di luar cakupan: prediksi harga, pembuktian sebab-akibat, dan faktor pendorong harga (kurs, cuaca, musim).

## Metodologi

**Pembersihan data PIHPS** (6 file, satu per komoditas, 31.038 baris setelah diubah ke format panjang)
- Header tanggal yang berbentuk teks (mis. `01/ 12/ 2023`) dan harga bertanda `Rp` atau pemisah ribuan dibersihkan otomatis.
- Duplikat: 0. Harga ≤ 0: 0.
- Sel kosong (kurang dari 1% per provinsi) diisi dengan harga sebelumnya, maksimal 3 hari berturut-turut. Total 92 sel diisi, tidak ada yang tersisa kosong.
- Outlier ditandai bila menyimpang lebih dari 50% dari median bergulir 15 hari. Ada 3 outlier (cabai merah keriting dan cabai rawit merah), ditandai tetapi **tidak dibuang**.
- Harga diagregasi ke bulanan (rata-rata harian), lalu dihitung perubahan bulanan (MoM) hanya bila bulan sebelumnya berurutan.

**Inflasi**
- 3 file BPS (2024, 2025, 2026) digabung, 152 entri wilayah (termasuk baris nasional "Indonesia").
- 38 kota dipetakan ke enam provinsi (Jawa Timur 11, Jawa Barat 10, Jawa Tengah 9, Banten 5, DI Yogyakarta 2, DKI Jakarta 1).
- Inflasi provinsi dihitung sebagai **rata-rata sederhana inflasi kota**, bukan angka resmi provinsi BPS.

**Analisis**
- Gejolak: simpangan baku MoM, jendela September 2025 – September 2026.
- Perubahan tahunan (YoY) per komoditas dan per provinsi.
- Posisi harga terhadap kebiasaan provinsi sendiri: persentase terhadap median, z-skor, dan persentil.
- Hubungan dengan inflasi: korelasi Spearman per pasangan komoditas × provinsi, indeks gabungan enam komoditas, dan uji jeda 1 bulan. Data gabungan: 1.152 baris (6 komoditas × 6 provinsi × 32 bulan).

## Temuan utama

| Komoditas | Gejolak (simpangan baku MoM, poin %) | YoY September 2026 (rata-rata 6 provinsi) |
|---|---:|---:|
| Cabai rawit merah | 36,2 | +91,5% |
| Cabai merah keriting | 20,1 | −8,9% |
| Bawang merah | 12,4 | −14,9% |
| Daging ayam | 8,0 | +6,8% |
| Telur ayam | 4,3 | −8,0% |
| Beras medium I | 0,8 | +3,6% |

**1. Lonjakan 2026 terkonsentrasi pada cabai rawit merah dan terjadi di keenam provinsi.**
Kenaikan tahunan tertinggi di Jawa Timur (+117,2%), DI Yogyakarta (+113,5%), dan Jawa Tengah (+97,7%). Dari Agustus ke September 2026, harganya naik 29,8% (Jawa Timur) hingga 40,9% (Jawa Tengah). Perlu dicatat: sebagian angka YoY berasal dari harga pembanding September 2025 yang rendah, dan rata-rata harga Jawa September 2026 (Rp73.002) masih 25,1% di bawah puncaknya pada Maret 2025.

**2. Dibanding kebiasaan provinsi sendiri, cabai rawit tinggi tetapi belum ekstrem.**
Harganya 21,6% (DKI Jakarta) hingga 35,9% (Jawa Tengah) di atas median provinsi, namun z-skornya 0,6–0,8 (persentil 74–79), masih dalam rentang gejolaknya. Jawa Tengah, Jawa Timur, dan DI Yogyakarta konsisten berada di tiga teratas, tetapi selisih antar-ketiganya hanya sekitar 3 poin persen sehingga tidak diurutkan tegas.

**3. Daging ayam dan beras tinggi karena naik bertahap, bukan melonjak.**
Z-skor keduanya 1,2–2,0 dan berada dekat titik tertinggi periode ini. Kenaikan bulanan September 2026 kecil: beras 0–2,3%, daging ayam −1,4% hingga +4,0%. Sebaliknya, bawang merah dan telur berada di bawah kebiasaannya (z-skor −0,3 hingga −1,3).

**4. Perubahan harga pangan berkorelasi positif dengan inflasi bulan yang sama.**
Korelasi Spearman rata-rata 0,37 (bawang merah 0,51 tertinggi, beras 0,17 terendah). Indeks gabungan enam komoditas berkorelasi 0,53–0,58 di semua provinsi (p < 0,01). Dengan jeda satu bulan, korelasi rata-rata mendekati nol (−0,15 hingga 0,21) dan tidak ada satu pun dari 36 pasangan yang signifikan (p terkecil 0,13), jadi tidak ada bukti bahwa kenaikan harga mendahului inflasi bulan berikutnya.

### Grafik dan tabel lengkap

Grafik tren, peta panas, dan tabel pendukung ada di notebook yang sudah dijalankan: [Buka di Google Colab](https://colab.research.google.com/drive/19QE4zUCXKTi_YnwAH7DSXmxFO-MZnDaU?usp=sharing)

## Keterbatasan

- Hanya enam provinsi di Pulau Jawa dan enam komoditas.
- Inflasi provinsi adalah rata-rata sederhana inflasi kota, bukan angka resmi BPS.
- Data inflasi berhenti di Agustus 2026, sehingga lonjakan harga September 2026 belum bisa dibandingkan dengan inflasinya.
- Hasil korelasi bukan bukti sebab-akibat. Keenam provinsi bergerak sangat serempak, jadi 36 pasangan komoditas × provinsi tidak diperlakukan sebagai bukti yang saling bebas. Komoditas pangan juga merupakan bagian dari penyusun inflasi, sehingga sebagian hubungan kemungkinan bersifat mekanis.
- Median dan z-skor dihitung dari seluruh periode (34 bulan) termasuk September 2026, dan seri harga yang punya tren naik (mis. beras) akan menghasilkan z-skor tinggi.
- Faktor pendorong harga (kurs, cuaca, musim) belum dianalisis.

## Notebook

- **Notebook (sudah dijalankan, lengkap dengan output dan grafik):** [Buka di Google Colab](https://colab.research.google.com/drive/19QE4zUCXKTi_YnwAH7DSXmxFO-MZnDaU?usp=sharing)
- **Data mentah dan hasil analisis (Google Drive):** [Buka folder](https://drive.google.com/drive/folders/1hfDEQRT8E3GYRYVJFxCHTWBY4Xoaa26Q?usp=sharing)

## Cara menjalankan ulang

Notebook dibuat di Google Colab dan membaca data dari Google Drive. Untuk sekadar melihat hasil, cukup buka tautan notebook di atas karena semua output sudah tersimpan.

Untuk menjalankan ulang:

1. Buka notebook, lalu **File → Save a copy in Drive**.
2. Unduh data mentah dari [folder Google Drive](https://drive.google.com/drive/folders/1hfDEQRT8E3GYRYVJFxCHTWBY4Xoaa26Q?usp=sharing) (atau langsung dari [PIHPS Bank Indonesia](https://www.bi.go.id/hargapangan) dan [BPS](https://www.bps.go.id)), lalu simpan di Google Drive-mu dengan struktur berikut. Folder `data/clean` dan `output` dibuat otomatis oleh notebook.
   ```
   MyDrive/proyek-harga-pangan/
   └── data/raw/
       ├── pihps/     (6 file .xlsx, satu per komoditas)
       └── inflasi/   (3 file .xlsx, satu per tahun)
   ```
   Jangan ubah nama file PIHPS, karena nama komoditas dibaca dari nama file.
3. Jalankan sel secara berurutan. Pustaka yang dibutuhkan sudah tersedia di Colab: `pandas`, `numpy`, `scipy`, `matplotlib`, `openpyxl`.

Parameter yang bisa diatur di sel setup: `BATAS_INFLASI`, `LIMIT_ISI` (maks. hari pengisian kosong), `AMBANG_OUTLIER`, dan `HAPUS_OUTLIER`.
