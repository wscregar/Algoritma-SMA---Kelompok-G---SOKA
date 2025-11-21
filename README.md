# Algoritma-SMA---Kelompok-G---SOKA

## Anggota Kelompok
| No | Nama | NRP |
|----|-------|-------|
| 1  |   Wira Samudra S    |  5027231041     |
| 2  |  Farand Febriansyah     |  5027231084   |
| 3  |  Muhammad Syahmi A    |   5027231085    |
| 4  |  Veri Rahman    |   5027231088    |
| 5. |  Abid Ubaidillah A  |   5027231089    |

# Pengujian Algoritma Task Scheduler pada Server IT

Repo ini merupakan kode dari server yang digunakan dalam pengujian Task Scheduling pada Server IT serta contoh algoritma scheduler untuk keperluan mata kuliah **Strategi Optimasi Komputasi Awan (SOKA)**

## Persiapan

1. Install `uv` sebagai dependency manager. Lihat [link berikut](https://docs.astral.sh/uv/getting-started/installation/)

2. Install semua requirement

```bash
uv sync
```

3. Buat file `.env` kemudian isi menggunakan variabel pada `.env.example`. Isi nilai setiap variabel sesuai kebutuhan

```conf
VM1_IP=""
VM2_IP=""
VM3_IP=""
VM4_IP=""

VM_PORT=5000
```

4. Kelompok kami menggunakan algortima ` Slime Mould Algorithm `

5. Untuk menjalankan server, jalankan docker

```bash
docker compose build --no-cache
docker compose up -d
```

6. Inisiasi Dataset untuk scheduler. Buat file `dataset.txt` kemudian isi dengan dataset berupa angka 1 - 10. Berikut adalah contohnya:

```txt
6
5
8
2
10
3
4
4
7
3
9
1
7
9
1
8
2
5
6
10
```

7. Untuk menjalankan scheduler, jalankan file `scheduler.py`. **Jangan lupa menggunakan VPN / Wifi ITS**

```bash
uv run scheduler.py
```

8. Apabila sukses, akan terdapat hasil berupa file `result.csv` dan pada console akan tampil perhitungan parameter untuk kebutuhan analisis.

# Algoritma Slime Module Algorithm

**Slime Mould Algorithm (SMA)** adalah algoritma optimasi yang meniru **perilaku jamur lendir (slime mould)** dalam mencari makanan. Jamur lendir mampu bergerak dan membentuk jalur paling efisien menuju sumber makanan — mirip proses mencari solusi terbaik.


## Cara Kerja SMA

Secara sederhana, SMA bekerja melalui tiga langkah utama:

### **1. Eksplorasi (Mencari Area Potensial)**

Seperti jamur lendir yang menyebar ke banyak arah untuk mencari makanan, SMA akan mengacak banyak “kandidat solusi” untuk menemukan area yang menjanjikan.

### **2. Eksploitasi (Memperkuat Jalur Terbaik)**

Ketika kandidat solusi mendekati hasil yang bagus, SMA akan:

* **Menguatkan** jalur menuju solusi tersebut.
* Menggerakkan kandidat lain untuk mengikuti arah yang sama.

Ini meniru bagaimana jamur lendir memperkuat jalur yang membawa lebih banyak nutrisi.

### **3. Pemilihan Solusi Terbaik**

Setelah beberapa iterasi, SMA menghasilkan **solusi paling optimal**, yaitu jalur paling efisien seperti perilaku jamur lendir menemukan makanan.

Dalam cloud computing, SMA dipakai untuk mengoptimalkan hal-hal seperti:

- Penjadwalan tugas (task scheduling

- Load balancing

- Resource allocation

- Energy optimization

## Alasan memakai SMA

* **Adaptif** → cepat menyesuaikan perubahan beban kerja.
* **Cepat menemukan solusi baik** di lingkungan yang kompleks.
* **Efisien** → meniru perilaku biologis yang sangat hemat energi.
* **Fleksibel** → cocok untuk beragam jenis optimasi.
  
# Teknis Uji Coba

Bagian ini memvalidasi bahwa pengujian antara SHC dan SMA dilakukan di bawah kondisi simulasi yang terkontrol, realistis, dan dapat dibandingkan.

1. Validasi Lingkungan Pengujian (Scheduler.py)

Kami memvalidasi bahwa sumber daya VM dan beban tugas didefinisikan secara realistis dan heterogen.Definisi Sumber Daya Heterogen:VM tidak seragam. Kami mendefinisikan 4 VM dengan spesifikasi CPU yang berbeda-beda: 1, 2, 4, dan 8 core (ditemukan di VM_SPECS pada scheduler.py).

Hal ini penting untuk menguji kemampuan algoritma dalam melakukan load balancing yang kompleks.

Beban Tugas Bervariasi:Tugas (Task) juga memiliki beban CPU yang bervariasi secara signifikan (berdasarkan formula (index * index * 10000)).


Ini mereplikasi skenario nyata di mana scheduler harus menangani tugas-tugas kecil dan heavy-duty secara bersamaan.

Validasi Kendala Kapasitas (Semaphore) 🚧Ini adalah bukti bahwa simulasi eksekusi tugas Anda tunduk pada kendala sumber daya fisik.
  
Implementasi Kendala CPU: Kami menggunakan objek asyncio.Semaphore di Python.Mekanisme: Semaphore pada setiap VM disetel sesuai dengan jumlah core CPU-nya (misalnya, VM4 (8 core) memiliki semaphore 8).

Fungsi: Tugas harus mendapatkan token dari semaphore VM-nya sebelum dapat dieksekusi. Jika 8 core VM4 sedang sibuk, tugas ke-9 harus menunggu (wait) hingga salah satu selesai.

Validasi: Mekanisme ini memvalidasi simulasi paralel dan memastikan Makespan yang diukur benar-benar mencerminkan kemacetan dan pemanfaatan sumber daya yang terjadi di cloud nyata.

3. Validasi Metrik dan Konsistensi Pengukuran

Kami memastikan bahwa pengukuran Makespan konsisten dan akurat.Model Makespan Bersama: Kedua algoritma (SHC dan SMA) menggunakan fungsi biaya yang identik (calculate_estimated_makespan di shc_algorithm.py).

Model ini menghitung waktu eksekusi sebagai: $\frac{\text{Beban Tugas}}{\text{Core VM}}$.

Makespan adalah nilai $\mathbf{\text{MAX}}$ dari total waktu yang dihabiskan semua VM.

Validasi: Memastikan bahwa perbandingan hasil (misalnya $15.22$ detik vs $15.4$ detik) adalah valid karena diukur menggunakan skala yang sama.

Pengukuran Waktu Akurat: Pengukuran waktu total dilakukan menggunakan time.monotonic() (di scheduler.py).

Validasi: Penggunaan monotonic memastikan bahwa waktu yang diukur kebal terhadap perubahan waktu sistem (misalnya jika jam sistem diubah), sehingga interval waktu eksekusi yang dicatat (Makespan) adalah seakurat mungkin.

4. Validasi Upaya Komputasi (Iterasi)Kami menetapkan batas iterasi yang jelas (SHC_ITERATIONS di scheduler.py): $\mathbf{1000}$ untuk SHC baseline dan $\mathbf{5000}$ untuk SMA yang dioptimalkan.

Validasi: Menetapkan upaya komputasi memvalidasi bahwa perbandingan dilakukan secara adil, di mana SMA (sebagai algoritma yang lebih kompleks) diberikan waktu pencarian yang lebih banyak untuk menunjukkan potensi superioritasnya.

# Pembahasan Hasil
