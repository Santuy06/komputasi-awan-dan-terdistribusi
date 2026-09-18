# Tugas 1 — Analisis Pitfall FoodGo

**Kelompok:** [nama kelompok]

| Nama | NIM | Kontribusi |
|---|---|---|
| Muhammad Fairuuz Dzakiy | 103072400120 | Pitfall 2 : Latency Is Zero |
| [nama 2] | [nim] | [pitfall/bagian yang dikerjakan] |
| Fathan Aditya Rachman | 103072400153 | Pitfall 3 : Single Point of Failure | 

## Pitfall 1: [nama pitfall] — ditulis oleh [nama]

**Bukti di skenario:** [kutip/paraphrase bagian skenario]

**Kenapa ini keliru:** [penjelasan]

**Dampak ke FoodGo:** [mekanisme kegagalan konkret]

**Solusi desain awal:** [usulan solusi]

**Trade-off:** [apa yang dikorbankan/risiko dari solusi ini]

---

## Pitfall 2: Latency Is Zero — ditulis oleh Muhammad Fairuuz Dzakiy

Bukti di skenario:
FoodGo tidak memiliki timeout pada pemanggilan antar-service. Modul pesanan memanggil modul pembayaran dan menunggu tanpa batas waktu.


Kenapa ini keliru:
Dalam sistem terdistribusi, komunikasi antar-service membutuhkan waktu. Respons dari service lain bisa mengalami keterlambatan karena beban server, jaringan, atau masalah pada service tujuan. Karena itu, sistem tidak boleh menganggap respons selalu datang secara langsung.

Dampak ke FoodGo:
Ketika modul pembayaran mengalami keterlambatan, request dari modul pesanan akan terus menunggu. Saat trafik meningkat, semakin banyak request yang tertahan. Resource seperti thread dan koneksi dapat semakin banyak digunakan sehingga aplikasi menjadi lambat dan beberapa request akhirnya timeout.

Solusi desain awal:
Gunakam timeout pada komunikasi antara modul pesanan dan pembayaran. FoodGo juga dapat menggunakan asynchronous processing atau message queue untuk proses yang tidak harus menunggu respons pembayaran secara langsung.


Trade-off:
Asynchronous processing dapat membuat sistem lebih kompleks. Status pesanan juga perlu dikelola karena hasil pembayaran bisa diterima setelah request awal selesai


## Pitfall 3: Single Point of Failure — ditulis oleh Fathan Aditya Rachman

Bukti di skenario:
Pada satu server disini bisa menjalankan semua modul yang ada pada FoodGo, seperti modul pesanan, pembayaran dan notifikasi kurir. ketiga modul tersebut berjalan di satu proses monilitik

Kenapa ini keliru:
Ketika semua fungsi bergantung pada satu server atau satu proses, kegagalan pada server tersebut bisa memengaruhi seluruh sistem. Beban yang terllau tinggi di satu modul dapat menghabiskan resource yang seharusnya digunakan oleh modul lain 

Dampak ke FoodGo:
Ketika trafik meningkat, server disini menangani semua tugas sekaligus, seperti menangani pesanan, pemabyaran dan notifikasi. Jika ini tidak di tangani server akan kewalahan dan dapat crash. Karena semua modul berada pada server yang sama, crash tersebtu membuat seluruh fungsi yang ada ikut berhenti dan membutuhkan restart manual 

Solusi desain awal:
Pisahkan modul yang ada seperti modul pesanan, pembayaran dan notifikasi agar service dapat berjlaan secara terpisah. Setiap service memiliki resource dan instance sendiri sehingga kegagaln atau beban tinggi pada satu service tidak langsung menghentikan service lainnya 

Trade-off:
Pemisahan server meningkatkan kimpleksitas sistem. Pada aplikasi Foodgo seharusnya menangani komunikasih antar-service, monitoring, deployment, dan kemungkinan kegagalan jaringan antar-service

## Kesimpulan Kelompok

[Ringkasan: jika FoodGo memperbaiki ketiga pitfall ini, apa arsitektur yang disarankan secara garis besar? Kaitkan dengan Tugas 2.]
