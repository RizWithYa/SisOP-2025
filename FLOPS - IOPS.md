# Mengukur FLOPS:

### FLOPS mengukur kemampuan prosesor dalam melakukan operasi titik mengambang per detik. Untuk menghitung FLOPS secara teoretis, kita dapat menggunakan rumus:​
---

**Jumlah Inti × Kecepatan Clock × Operasi per Siklus**

---
Misalnya jika kita mempunyai sebuah CPU yang memiliki 8 Core dengan kecepatan sebesar 5.1 GHz dan 8 Operasi per siklus, maka untuk perhitungannya adalah :

## 8 x 5.1 GHz x 8 = 326.4 GFLOPS
<br>
Kurang lebih untuk cara teoritis untuk menghitung FLOPS pada CPU kita, untuk lebih lanjut dapat menggunakan aplikasi benchmark yang sudah tersebar di Internet.


___


# Mengukur IOPS:

### IOPS mengukur jumlah operasi input/output yang dapat ditangani oleh perangkat penyimpanan per detik. Untuk mengukur IOPS, Anda dapat menggunakan alat seperti FIO (Flexible I/O Tester) atau Aplikasi benchmark lainnya seperti crystalbenchmark. ###

<p>Pada kali ini, kita akan mengetes Benchmarking pada Ubuntu menggunakan FIO, Untuk cara penginstalannya seperti langkah berikut :</p>

---

### Ubuntu/Debian
```bash
sudo apt-get update
sudo apt-get install fio
```

---

#### Setelah melakukan instalasi, maka kita dapat menjalankan benchmarking menggunakan FIO pada Ubuntu. Untuk melakukan benchmarking, maka dapat dilakukan dengan command berikut ini : ####

```bash
fio --randrepeat=1 --ioengine=libaio --direct=1 --gtod_reduce=1 --name=test --filename=test --bs=4k --iodepth=64 --size=4G --readwrite=randrw --rwmixread=75
```

<p>Command ini akan melakukan pengujian baca/tulis acak dengan ukuran blok 4K dan rasio baca 75%.</p>

<br>

<img src="https://github.com/RizWithYa/SisOP-2025/blob/main/IOPS.png?raw=true" placeholder="Foto Test IOPS Ubuntu">
