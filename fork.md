# Penjelasan Mengenai Forking 

<br>

## Apa Itu Forking?

Forking merupakan sebuah mekanisme di mana suatu proses induk (parent process) membuat sebuah duplikat dari dirinya sendiri, yang kemudian dikenal sebagai proses anak (child process). Proses ini diinisiasi melalui sebuah panggilan sistem (system call) bernama `fork()` dalam bahasa pemrograman C. Setelah `fork()` dieksekusi, akan ada dua proses yang berjalan secara terpisah: proses induk dengan identitas (ID) aslinya, dan proses anak yang mendapatkan ID baru. Kedua proses ini akan melanjutkan eksekusi program dari titik tepat setelah pemanggilan `fork()`.

<br>

## Implementasi Forking dalam Bahasa C

Berikut adalah contoh kode untuk melakukan forking di C:

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    pid_t pid = fork(); // Memanggil fork untuk membuat proses anak

    if (pid < 0) { // Jika pid bernilai negatif
        perror("Fork gagal"); // Menampilkan pesan kesalahan
        return 1; // Mengindikasikan kegagalan
    } else if (pid == 0) { // Jika pid bernilai 0, ini adalah proses anak
        printf("Ini adalah proses anak dengan PID %d\n", getpid());
    } else { // Jika pid bernilai positif, ini adalah proses induk
        printf("Ini adalah proses induk dengan PID %d dan anak dengan PID %d\n", getpid(), pid);
    }

    return 0; // Mengindikasikan eksekusi sukses
}
```
Penjelasan Contoh:
 * Panggilan fork() digunakan untuk menciptakan proses anak.
 * Nilai pid yang negatif menandakan bahwa proses forking tidak berhasil.
 * Jika pid bernilai 0, berarti segmen kode tersebut sedang dijalankan oleh proses anak.
 * Jika pid bernilai positif, segmen kode tersebut dijalankan oleh proses induk, dan nilai pid tersebut merupakan ID dari proses anak yang baru dibuat.
<br>
Apakah Kode print123thread.c Menggunakan Forking?
Tidak. Berdasarkan analisis, kode yang terdapat pada tautan print123thread.c tidak menggunakan mekanisme forking. Sebaliknya, kode tersebut memanfaatkan threading menggunakan pustaka pthread untuk menjalankan tiga thread secara tersinkronisasi.
<br>
<h4>Analisis Kode print123thread.c</h4>
<h5>Setelah ditinjau, kode pada print123thread.c tidak menggunakan fork(), melainkan mengimplementasikan threading dengan pthread. Tujuannya adalah untuk mencetak angka 1, 2, dan 3 secara berurutan dan terus-menerus.</h5> 
---
Berikut adalah rincian cara kerjanya:
 * Inisialisasi Program: Kode memulai dengan mengimpor pustaka-pustaka yang dibutuhkan. Selanjutnya, variabel-variabel global dideklarasikan, termasuk sebuah mutex (pthread_mutex_t) dan variabel-variabel kondisi (pthread_cond_t) yang esensial untuk sinkronisasi antar thread.
   
 * Variabel Global yang Digunakan:
   
   * pthread_mutex_t lock: Sebuah mutex yang berfungsi untuk melindungi dari race condition (kondisi balapan) saat mengakses sumber daya bersama.
     
   * pthread_cond_t cond1, cond2, cond3: Variabel kondisi yang digunakan untuk mengatur urutan eksekusi antar thread.
     
   * int done = 1: Variabel ini berperan sebagai penanda untuk menentukan thread mana yang seharusnya aktif dan berhak mencetak angka.

---
Fungsi Thread (foo):
   * Fungsi ini dirancang untuk dieksekusi oleh setiap thread dan menerima sebuah argumen berupa pointer ke integer, yang merepresentasikan nomor identifikasi thread (antara 1, 2, atau 3).
     
   * Di dalam sebuah loop yang berjalan tanpa henti, setiap thread akan berusaha untuk mengunci mutex.
     
   * Jika nilai variabel done tidak cocok dengan nomor thread yang sedang berjalan, thread tersebut akan menunggu (masuk kondisi wait) pada variabel kondisi yang telah ditentukan untuknya.
     
   * Apabila nilai done sesuai dengan nomor thread, thread tersebut akan mencetak nomornya, kemudian memperbarui nilai done untuk mengaktifkan thread berikutnya, dan mengirimkan sinyal ke variabel kondisi milik thread berikutnya tersebut.
     
   * Setelah operasi selesai, mutex akan dilepas.
---
Fungsi Utama (main):
   
   * Fungsi main bertanggung jawab untuk membuat tiga buah thread (tid1, tid2, tid3). Masing-masing thread ini akan menjalankan fungsi foo dengan argumen yang berbeda (1, 2, dan 3).
   
   * Thread utama kemudian akan menunggu hingga ketiga thread yang telah dibuat tersebut selesai dieksekusi (meskipun dalam kasus ini mereka berjalan dalam loop tak terbatas) dengan menggunakan panggilan pthread_join.

