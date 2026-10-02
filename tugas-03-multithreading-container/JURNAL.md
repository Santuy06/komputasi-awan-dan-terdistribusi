# Jurnal Proses — Tugas 3

## Percobaan tanpa Lock

* Hasil `processed_count` yang didapat: **39 dari 100 pesanan**.

* Kenapa bisa meleset (jelaskan mekanisme race condition dengan kata sendiri):
  Race condition terjadi karena beberapa thread mengakses dan mengubah `processed_count` secara bersamaan tanpa pengaman. Dua atau lebih thread dapat membaca nilai `processed_count` yang sama sebelum salah satunya selesai melakukan perubahan. Akibatnya, hasil penambahan dari salah satu thread dapat tertimpa oleh thread lain sehingga jumlah akhir menjadi kurang dari jumlah pesanan yang sebenarnya diproses.

## Percobaan dengan Lock

* Hasil `processed_count` setelah perbaikan: **100 dari 100 pesanan**.

* Perbaikan dilakukan dengan menggunakan `threading.Lock()`. Proses perubahan nilai `processed_count` diletakkan di dalam `with lock:` sehingga hanya satu thread yang dapat melakukan increment pada satu waktu. Dengan demikian, setiap pesanan dapat dihitung dengan benar.

## Kendala Docker

* Error yang ditemui saat `docker build`/`docker run` dan cara memperbaikinya:
  Pada percobaan awal, terdapat kesalahan saat menjalankan perintah `cd tugas-03-multithreading-container` karena terminal Codespaces sudah berada di dalam folder `tugas-03-multithreading-container`. Perintah `cd` tersebut kemudian tidak diperlukan.

* Setelah itu, proses Docker dapat dijalankan dari direktori proyek dengan perintah:

```bash
docker build -t foodgo-order-sim .
docker run --rm foodgo-order-sim
```

* Container berhasil menjalankan program dan menghasilkan `processed_count` sebesar **100 dari 100 pesanan**.

## Log Penggunaan AI (Level 2)

> Wajib diisi sesuai kebijakan Level 2 di [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md).

| Tanggal    | Tool AI | Prompt yang diberikan                                                                                    | Ringkasan saran/ide AI                                                                                                                                | Bagaimana diolah jadi tulisan/kode sendiri                                                                                                                          |
| ---------- | ------- | -------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 02-10-2026 | ChatGPT | Meminta bantuan memahami tugas simulasi pesanan dengan multithreading, race condition, Lock, dan Docker. | AI memberikan arahan mengenai tahapan pengerjaan, cara menguji kondisi tanpa Lock dan dengan Lock, serta cara menjalankan program menggunakan Docker. | Saya memahami kembali konsep dan langkah pengerjaan, kemudian menyesuaikan implementasi dan hasil percobaan dengan kode serta lingkungan Codespaces yang digunakan. |
