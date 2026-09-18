# Tugas 1 — Analisis Pitfall FoodGo

**Kelompok:** [nama kelompok]

| Nama | NIM | Kontribusi |
|---|---|---|
| Muhammad Fairuuz Dzakiy | 103072400120 | Pitfall 2 : Latency Is Zero |
| Daud Achmad | 103072400141 | Pitfall 1 : The network is reliable |
| Fathan Aditya Rachman | 103072400153 | Pitfall 3 : Single Point of Failure | 

## Pitfall 1: The network is reliable — ditulis oleh Daud Achmad 

**Bukti di skenario:** **“Network is always reliable, no need for retry.”**, dan **“Tidak ada timeout sama sekali pada pemanggilan antar service.”**

**Kenapa ini keliru:** Asumsi bahwa jaringan selalu andal dan tanpa hambatan adalah sebuah kekeliruan dalam komputasi terdistribusi. Pada kenyataannya komunikasi antar service melalui jaringan tidak selalu dapat diprediksi. Request bisa gagal di tengah jalan, koneksi bisa terputus secara tiba tiba, atau service tujuan bisa mengalami hang sehingga tidak memberikan respons.

**Dampak ke FoodGo:** Jika modul pembayaran mengalami gangguan atau lambat dan tidak merespons, modul pesanan akan terus menunggu tanpa batas waktu. Ketika volume pemesanan makanan meningkat, antrean request baru akan terus menumpuk di modul pesanan. Hal ini dapat menghabiskan resource server seperti thread pool dan memori. Akibatnya aplikasi FoodGo menjadi sangat lambat hingga akhirnya server mengalami crash dan sistem tumbang total.

**Solusi desain awal:** 
- Terapkan batasan waktu tunggu atau timeout yang ketat pada setiap pemanggilan API antar service untuk membebaskan resource yang tertahan.
- Implementasikan mekanisme coba ulang otomatis atau retry dengan jeda waktu yang semakin meningkat atau exponential backoff untuk mengatasi kegagalan jaringan yang bersifat sementara.
- Gunakan pola 'circuit breaker' untuk langsung memutus aliran request ke modul pembayaran jika terdeteksi gagal terus-menerus sehingga modul pesanan terlindungi dari beban berlebih.

**Trade-off:** 
- Mekanisme retry berpotensi menambah kepadatan lalu lintas jaringan dan memperberat beban kerja pada server tujuan yang mungkin sedang bermasalah.
- Ada risiko terjadinya pembayaran ganda jika proses pembayaran di backend sebenarnya sukses, tetapi respons sukses tersebut gagal diterima oleh modul pesanan akibat gangguan jaringan.
- Untuk memitigasi risiko pembayaran ganda, sistem wajib mengimplementasikan mekanisme Idempotency, misalnya menggunakan Idempotency Key yang menambah kompleksitas pada kode program dan basis data.

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
