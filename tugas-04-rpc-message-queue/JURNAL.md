# Jurnal Proses — Tugas 4

## Jalur yang dipilih

- Keduanya, RPC dan MQ. RPC digunakan untuk komunikasi sinkron antara modul Pesanan dan Pembayaran, misalnya saat mengecek saldo dan memproses pembayaran. Sedangkan MQ digunakan untuk mengirim notifikasi secara asinkron, jadi modul Pembayaran tidak perlu menunggu modul Kurir selesai memproses pesan.

## Kendala teknis

- Kami sempat mengalami masalah pada virtual environment (venv) karena library `pika` tidak ditemukan saat menjalankan program. Error yang muncul adalah `ModuleNotFoundError: No module named 'pika'`. Untuk mengatasinya, kami perlu memastikan virtual environment yang digunakan sudah benar dan menginstal dependensi melalui `python -m pip install -r requirements.txt`.

## Uji "pesan tidak hilang" (khusus Jalur B)

- Langkah uji: Kami mematikan consumer terlebih dahulu, kemudian menjalankan publisher untuk mengirim pesan. Setelah itu, consumer dijalankan kembali untuk melihat apakah pesan yang sudah dikirim masih bisa diterima.
- Hasil yang diamati: Pada jalur RPC, client berhasil menerima respons dari server untuk pengecekan saldo dan pemrosesan pembayaran. Pada jalur MQ, publisher berhasil mengirim tiga pesan pembayaran, lalu consumer menerima dan memproses notifikasi dengan jumlah Rp20.000, Rp40.000, dan Rp60.000. Untuk memastikan pesan tidak hilang saat consumer mati, hasil pengujian perlu diperiksa berdasarkan pesan yang diterima setelah consumer dijalankan kembali.

## Log Penggunaan AI (Level 2)

> Wajib diisi sesuai kebijakan Level 2 di [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md). Tulis "Tidak memakai AI" pada baris pertama jika memang tidak dipakai. Hanya untuk brainstorming ide/outline — bukan untuk kode/analisis/teks akhir.

| **Tanggal** | **Tool AI** | **Prompt yang diberikan** | **Ringkasan saran/ide AI** | **Bagaimana diolah jadi tulisan/kode sendiri** |
| ----------- | ----------- | ------------------------- | -------------------------- | ---------------------------------------------- |
| 11 Oktober 2026 | ChatGPT | Membantu memahami tugas komunikasi antar komponen, RPC dan Message Queue, serta menjelaskan bagian TODO yang perlu diselesaikan. | Menjelaskan perbedaan komunikasi sinkron melalui RPC dan asinkron melalui Message Queue, serta membantu mengidentifikasi bagian kode yang perlu dilengkapi. | Memahami fungsi setiap komponen, mempelajari implementasi RPC dan MQ, lalu meninjau kode yang digunakan dalam tugas. |
| 11 Oktober 2026 | ChatGPT | Membantu mengatasi error `ModuleNotFoundError: No module named 'pika'` saat menjalankan consumer dan publisher. | Menyarankan pembuatan ulang virtual environment dan instalasi dependensi dari `requirements.txt`. | Menjalankan perintah terminal untuk memperbaiki environment dan memasang library `pika` agar program dapat dijalankan. |
| 11 Oktober 2026 | ChatGPT | Membantu menyusun README.md dan JURNAL.md untuk tugas RPC dan Message Queue. | Memberikan kerangka analisis pemilihan pola komunikasi, catatan kendala teknis, dan dokumentasi hasil pengujian. | Menyesuaikan dokumentasi dengan implementasi, kendala, dan hasil pengujian program yang dikerjakan. |
| 11 Oktober 2026 | ChatGPT | Membantu menyiapkan perintah Git untuk commit dan push tugas ke GitHub. | Menyarankan pemeriksaan perubahan, staging folder tugas, pembuatan commit, dan push ke branch `main`. | Memeriksa perubahan file dan menjalankan perintah Git untuk mengirim hasil pengerjaan ke repository kelompok. |
