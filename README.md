# Algoritma-SMA---Kelompok-G---SOKA

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

# Pembahasan Hasil
