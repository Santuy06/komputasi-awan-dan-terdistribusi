# Jurnal Proses — Tugas 4

## Jalur yang dipilih
Saya memilih Jalur B, yaitu Message Queue menggunakan RabbitMQ. 
Saya memilih jalur ini karena ingin memahami bagaimana modul pembayaran bisa mengirim notifikasi 
tanpa harus menunggu modul kurir selesai memprosesnya.

Pada program ini, publisher.py berperan sebagai modul pembayaran yang mengirimkan data ke antrean pembayaran_berhasil. 
Sementara itu, consumer.py berperan sebagai modul kurir yang 
menerima dan memproses pesan tersebut.

Dengan cara ini, kedua modul tidak harus aktif dan bekerja pada waktu yang sama. Menurut saya, 
pendekatan ini cocok untuk notifikasi pembayaran karena proses pembayaran tidak perlu menunggu respons dari modul kurir.

## Kendala Teknis
Selama mengerjakan tugas, saya perlu memahami cara menghubungkan program Python dengan RabbitMQ. 
Program menggunakan library pika untuk melakukan komunikasi dengan message broker.

Saya juga perlu memastikan bahwa publisher dan consumer menggunakan nama antrean yang sama, 
yaitu pembayaran_berhasil. Jika nama antreannya berbeda, pesan yang dikirim tidak akan diterima oleh consumer yang diharapkan.

Selain itu, RabbitMQ perlu dijalankan terlebih dahulu menggunakan Docker sebelum program publisher dan consumer digunakan. 
Pengaturan antrean dengan durable=True dan pengiriman pesan dengan delivery_mode=2 juga digunakan agar antrean dan pesan dapat bertahan 
ketika RabbitMQ mengalami restart, dengan tetap memperhatikan kondisi dan konfigurasi broker.

## Uji "pesan tidak hilang" (khusus Jalur B)
Pengujian dilakukan dengan menjalankan consumer dan publisher melalui dua terminal. Consumer dijalankan untuk menerima pesan dari 
antrean, sedangkan publisher digunakan untuk mengirimkan tiga event pembayaran.

Setiap pesan berisi user_id, jumlah, dan timestamp. Berdasarkan hasil pengujian, publisher berhasil mengirimkan tiga event untuk 
user1, user2, dan user3. Consumer kemudian menampilkan notifikasi pembayaran dengan jumlah masing-masing Rp20.000, Rp40.000, dan Rp60.000.

Untuk menguji apakah pesan tetap tersimpan, consumer perlu dihentikan terlebih dahulu, kemudian publisher dijalankan untuk mengirim pesan. 
Setelah itu, consumer dijalankan kembali. Pesan yang belum diproses seharusnya tetap berada dalam antrean dan dapat diterima ketika consumer aktif kembali.

Pengujian tersebut menunjukkan manfaat message queue, yaitu pengiriman pesan tidak harus menunggu consumer tersedia. 
Namun, pengujian perlu benar-benar dilakukan sebelum hasilnya dinyatakan berhasil. 
Penggunaan antrean durable dan pesan persisten juga tidak menjamin semua kondisi kegagalan dapat ditangani tanpa konfigurasi tambahan.

## Log Penggunaan AI (Level 2)

> Wajib diisi sesuai kebijakan Level 2 di [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md). Tulis "Tidak memakai AI" pada baris pertama jika memang tidak dipakai. Hanya untuk brainstorming ide/outline — bukan untuk kode/analisis/teks akhir.

| Tanggal    | Tool AI | Prompt yang diberikan                                                                                    | Ringkasan saran/ide AI                                                                                                                                | Bagaimana diolah jadi tulisan/kode sendiri                                                                                                                          |
| ---------- | ------- | -------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 11-11-2026 | ChatGPT | Meminta bantuan menyusun jurnal proses tugas Message Queue berdasarkan instruksi tugas dan kode publisher serta consumer. |Membantu menyusun kerangka jurnal yang mencakup alasan pemilihan jalur, kendala teknis, dan pengujian pesan. | Saya memeriksa kembali isi jurnal dengan kode dan hasil pengujian yang saya jalankan, lalu menyesuaikan penjelasan dengan proses pengerjaan saya sendiri. |
