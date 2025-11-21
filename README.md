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

![WhatsApp Image 2025-11-21 at 10 39 19_29eec2d4](https://github.com/user-attachments/assets/83c58a18-0407-43ec-894d-5306635584e2)

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

## 1. Validasi Lingkungan Pengujian (Scheduler.py)

<img width="513" height="129" alt="Screenshot 2025-11-21 100717" src="https://github.com/user-attachments/assets/f2e0dbc6-45ea-4b76-a7a5-748a4d6f006d" />

Kami memvalidasi bahwa sumber daya VM dan beban tugas didefinisikan secara realistis dan heterogen.Definisi Sumber Daya Heterogen:VM tidak seragam. 

Kami mendefinisikan 4 VM dengan spesifikasi CPU yang berbeda-beda: 1, 2, 4, dan 8 core.

Hal ini penting untuk menguji kemampuan algoritma dalam melakukan load balancing yang kompleks.

<img width="318" height="75" alt="Screenshot 2025-11-21 100809" src="https://github.com/user-attachments/assets/97c22e80-85fd-4377-8d36-ecb324484666" />

Beban Tugas Bervariasi:Tugas (Task) juga memiliki beban CPU yang bervariasi secara signifikan (berdasarkan formula (index * index * 10000)).

Ini mereplikasi skenario nyata di mana scheduler harus menangani tugas-tugas kecil dan heavy-duty secara bersamaan.

## 2. Validasi Kendala Kapasitas (Semaphore)

Ini adalah bukti bahwa simulasi eksekusi tugas Anda tunduk pada kendala sumber daya fisik.
  
Implementasi Kendala CPU: Kami menggunakan objek `asyncio.Semaphore` di Python.

<img width="668" height="58" alt="Screenshot 2025-11-21 100912" src="https://github.com/user-attachments/assets/9c806a29-e735-4a55-85eb-636e267c7e48" />

Mekanisme: Semaphore pada setiap VM disetel sesuai dengan jumlah core CPU-nya (misalnya, VM4 (8 core) memiliki semaphore 8).

<img width="578" height="60" alt="Screenshot 2025-11-21 100923" src="https://github.com/user-attachments/assets/45e5ec48-05c1-48a4-8603-fec97b5e4679" />

Fungsi: Tugas harus mendapatkan token dari semaphore VM-nya sebelum dapat dieksekusi. Jika 8 core VM4 sedang sibuk, tugas ke-9 harus menunggu (wait) hingga salah satu selesai.

Validasi: Mekanisme ini memvalidasi simulasi paralel dan memastikan Makespan yang diukur benar-benar mencerminkan kemacetan dan pemanfaatan sumber daya yang terjadi di cloud nyata.

## 3. Validasi Metrik dan Konsistensi Pengukuran

Kami memastikan bahwa pengukuran Makespan konsisten dan akurat.Model Makespan Bersama: Kedua algoritma (SHC dan SMA) menggunakan fungsi biaya yang identik (calculate_estimated_makespan di shc_algorithm.py).

<img width="751" height="302" alt="Screenshot 2025-11-21 101020" src="https://github.com/user-attachments/assets/59084f5b-6428-4057-ac14-d18b5941a7c0" />

Model ini menghitung waktu eksekusi sebagai: $\frac{\text{Beban Tugas}}{\text{Core VM}}$.

Makespan adalah nilai $\mathbf{\text{MAX}}$ dari total waktu yang dihabiskan semua VM.

Validasi: Memastikan bahwa perbandingan hasil (misalnya $15.22$ detik vs $15.4$ detik) adalah valid karena diukur menggunakan skala yang sama.

Pengukuran Waktu Akurat: Pengukuran waktu total dilakukan menggunakan time.monotonic() (di scheduler.py).

Validasi: Penggunaan monotonic memastikan bahwa waktu yang diukur kebal terhadap perubahan waktu sistem (misalnya jika jam sistem diubah), sehingga interval waktu eksekusi yang dicatat (Makespan) adalah seakurat mungkin.

### Implementasi Algoritma Slime Mould (SMA)

Implementasi SMA dapat dibagi menjadi empat tahap utama yang mereplikasi perilaku pencarian jamur lendir.

####  Setup & Encoding

Populasi Awal (population):

<img width="273" height="98" alt="Screenshot 2025-11-21 104313" src="https://github.com/user-attachments/assets/fd4eb02a-aa5d-4bbf-af37-730cb8b3aa5b" />

menetapkan jumlah solusi yang dicari. Ini seperti 50 "tubuh" jamur lendir yang mencari makan.

<img width="640" height="45" alt="Screenshot 2025-11-21 104343" src="https://github.com/user-attachments/assets/1c9fe887-6e9f-428a-9655-cb4f848e009a" />

membuat solusi acak awal, di mana setiap tugas (kolom) diberi VM (nilai 0 hingga 3).

Evaluasi Awal (fitness) :

<img width="816" height="302" alt="image" src="https://github.com/user-attachments/assets/1f1463f4-dbd5-45af-8277-2f47984b1228" />

digunakan untuk mengevaluasi Makespan (kualitas makanan) dari setiap solusi awal

<img width="390" height="64" alt="image" src="https://github.com/user-attachments/assets/0a82c3b5-5dfd-4c4c-baf5-13134519120e" />

mencatat Makespan terbaik yang ditemukan sejauh ini.

## 4. Validasi Upaya Komputasi (Iterasi)

Kami menetapkan batas iterasi yang jelas (SHC_ITERATIONS di scheduler.py): $\mathbf{1000}$ untuk SHC baseline dan $\mathbf{5000}$ untuk SMA yang dioptimalkan.

<img width="193" height="36" alt="image" src="https://github.com/user-attachments/assets/3426eb0d-b57a-466e-82f4-7035605e6b7c" />

Validasi: Menetapkan upaya komputasi memvalidasi bahwa perbandingan dilakukan secara adil, di mana SMA (sebagai algoritma yang lebih kompleks) diberikan waktu pencarian yang lebih banyak untuk menunjukkan potensi superioritasnya.

# Pembahasan Hasil

## Ringkasam Hasil

Secara umum, algoritma SMA berhasil melakukan penjadwalan tugas (task scheduling) dengan sangat stabil dan efisien.

* Konsistensi Tinggi: Ketiga pengujian menghasilkan total waktu penyelesaian (Makespan) yang hampir identik, yaitu sekitar 25.7 detik.

* Alokasi Cerdas: Algoritma secara cerdas membebankan tugas terbanyak ke VM dengan spesifikasi tertinggi (VM4, 8 Core) dan tugas paling sedikit ke VM terlemah (VM1, 1 Core).

* Efisiensi Waktu Tunggu: Rata-rata waktu tunggu (wait time) relatif rendah, terutama pada pengujian ketiga yang mencapai angka terbaik (1.5 detik).

## Perbandingan Hasil Pengujian (Metrik Utama)

| Metrik | Revisi 1 (sma_results_revisi) | Revisi 2 (sma_results_revisi2) | Revisi 3 (sma_results_revisi3) |
|----|-------|-------|-------|
| Makespan (Waktu Selesai Total) |  25.66 detik	| 25.75 detik |	25.70 detik |
| Rata-rata Wait Time   |  3.15 detik |	3.05 detik |	1.54 detik (Terbaik) |
| Total Beban Eksekusi (Semua Tugas) |  229.6 detik |	248.3 detik |	221.1 detik |
| VM Tersibuk  |  VM4 (11 tugas) |	VM4 (9 tugas) |	VM4 (8 tugas) |

## Analisis Alokasi Load Balancing
Algoritma SMA dirancang untuk meminimalkan waktu selesai dengan memanfaatkan spesifikasi VM. Berdasarkan kode scheduler.py, spesifikasi VM adalah:

* VM1: 1 Core

* VM2: 2 Core

* VM3: 4 Core

* VM4: 8 Core

Data distribusi beban dari hasil tes menunjukkan algoritma bekerja sesuai logika kapasitas:

Contoh Distribusi (dari Revisi 1):

<img width="738" height="874" alt="image" src="https://github.com/user-attachments/assets/d68cef08-9a6c-4b2a-8954-f9af408b6bd1" />


* VM4 (8 Core): Menangani 11 tugas dengan total durasi beban kerja 120 detik. Namun, karena memiliki 8 core, ia dapat memproses banyak tugas secara paralel, sehingga selesai dalam waktu 25 detik.

* VM1 (1 Core): Hanya menangani 4 tugas dengan total beban 22 detik.

* VM2 & VM3: Mendapat porsi menengah (1-4 tugas).

Kesimpulan: Algoritma tidak membagi tugas secara "rata jumlahnya", melainkan rata kinerjanya. Jika tugas dibagi rata secara jumlah (misal semua dapat 5 tugas), VM1 akan kewalahan dan membuat sistem lambat (bottleneck), sementara VM4 akan menganggur. SMA menghindari hal ini dengan memberikan VM4 pekerjaan 2-3x lipat lebih banyak.

# Analisis Kinerja Algoritma Slime Mould (SMA)

Dokumen ini menyajikan analisis hasil pengujian 10 kali (runs) pada Algoritma Slime Mould (SMA) untuk penjadwalan tugas di lingkungan Virtual Machine (VM) Heterogen.

## 1. Parameter Uji Coba

| Parameter | Nilai | Keterangan |
| :--- | :--- | :--- |
| **Algoritma** | SMA (Modifikasi Konsistensi) | Dilengkapi logika pergerakan acak eksplosif (noise). |
| **Jumlah Iterasi** | 5000 | Waktu pencarian yang cukup panjang. |
| **Ukuran Populasi** | 50 | Diversitas solusi awal yang besar. |
| **Lingkungan VM** | 4 VM (1, 2, 4, 8 Core) | Heterogenitas sumber daya yang tinggi. |
| **Jumlah Tugas** | 20 Tugas | Beban tugas yang bervariasi (Load $\propto Index^2$). |

## 2. Hasil Rata-Rata (10x Run)

| Parameter | Total Nilai (Sum) | Rata-Rata ($\bar{x}$) | Satuan |
| :--- | :---: | :---: | :--- |
| **Makespan** (Waktu Total) | 261.9830 | **26.1983** | detik |
| **Throughput** | 7.6400 | **0.7640** | tugas/detik |
| **Total CPU Time** | 243.0170 | **24.3017** | detik |
| **Total Wait Time** | 453.7140 | **45.3714** | detik |
| **Resource Utilization (CPU)** | 612.165% | **61.2165%** | % |
| **Imbalance Degree** | 15.3447 | **1.5345** | (tanpa satuan) |

## 3. Pembahasan Hasil

Hasil rata-rata 10 kali run menunjukkan bahwa SMA memiliki **potensi optimalitas tinggi** (terbukti dari hasil terbaik individu $\approx 15.22$ detik), namun memiliki **konsistensi yang rendah** (terbukti dari Makespan rata-rata $26.1983$ detik).

### A. Dominasi Imbalance Degree terhadap Makespan

Faktor utama yang menyebabkan Makespan rata-rata tinggi adalah **Imbalance Degree yang tinggi ($\mathbf{1.5345}$)** dan Total Wait Time yang signifikan ($\mathbf{45.37}$ detik).

* **Penyebab:** Ketika algoritma gagal mendistribusikan beban tugas yang sangat bervariasi (berbanding lurus dengan $Index^2$), beberapa VM (terutama yang memiliki *core* lebih sedikit) menjadi sangat jenuh, sementara VM lainnya menganggur. Waktu tunggu ini secara langsung menaikkan Makespan.
* **Kesimpulan:** Logika SMA, bahkan dengan penambahan *noise* (L. 133-135 pada `shc_algorithm.py`), belum cukup efisien untuk mengatasi kerumitan penjadwalan di lingkungan heterogen yang memiliki **kendala Makespan**.

### B. Tingkat Pemanfaatan Sumber Daya

Resource Utilization sebesar $\mathbf{61.21\%}$ menunjukkan bahwa rata-rata $\approx 38.79\%$ kapasitas CPU total terbuang karena waktu tunggu. Ini mendukung argumen bahwa **penempatan tugas tidak seimbang**, mengakibatkan terbuangnya kapasitas dari VM yang kosong sementara VM lain terlalu sibuk.
