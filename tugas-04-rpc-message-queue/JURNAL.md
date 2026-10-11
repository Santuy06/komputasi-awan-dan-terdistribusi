# Jurnal Proses — Tugas 4

## Jalur yang dipilih

- Keduanya, RPC dan MQ. RPC digunakan untuk komunikasi sinkron antara modul Pesanan dan Pembayaran, misalnya saat mengecek saldo dan memproses pembayaran. Sedangkan MQ digunakan untuk mengirim notifikasi secara asinkron, jadi modul Pembayaran tidak perlu menunggu modul Kurir selesai memproses pesan.

## Kendala teknis

- Kami sempat mengalami masalah pada virtual environment (venv) karena library `pika` tidak ditemukan saat menjalankan program. Error yang muncul adalah `ModuleNotFoundError: No module named 'pika'`. Untuk mengatasinya, kami perlu memastikan virtual environment yang digunakan sudah benar dan menginstal dependensi melalui `python -m pip install -r requirements.txt`.

## Uji "pesan tidak hilang" (khusus Jalur B)

- Langkah uji: Kami mematikan consumer terlebih dahulu, kemudian menjalankan publisher untuk mengirim pesan. Setelah itu, consumer dijalankan kembali untuk melihat apakah pesan yang sudah dikirim masih bisa diterima.
- Hasil yang diamati: Pada jalur RPC, client berhasil menerima respons dari server untuk pengecekan saldo dan pemrosesan pembayaran. Pada jalur MQ, publisher berhasil mengirim tiga pesan pembayaran, lalu consumer menerima dan memproses notifikasi dengan jumlah Rp20.000, Rp40.000, dan Rp60.000. Untuk memastikan pesan tidak hilang saat consumer mati, hasil pengujian perlu diperiksa berdasarkan pesan yang diterima setelah consumer dijalankan kembali.

| Tanggal | Tool AI | Prompt yang diberikan | Ringkasan saran/ide AI | Bagaimana diolah jadi tulisan/kode sendiri |
|---|---|---|---|---|
| 11 Oktober 2026 | ChatGPT | Bedanya RPC sama message queue itu apa, terus cara kerjanya di RabbitMQ gimana? | Mendapat penjelasan tentang perbedaan RPC dan message queue, termasuk cara kerja publisher dan consumer. | Kami membaca penjelasannya, lalu mencocokkannya dengan kode yang digunakan supaya lebih paham cara kerja programnya. |
