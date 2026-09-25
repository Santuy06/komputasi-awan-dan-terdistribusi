# Tugas 2 (Pekan 2) — Perancangan Arsitektur untuk FoodGo

**Materi terkait:** Architectural style (Layered, SOA, Peer-to-Peer, Publish-Subscribe).

## Studi Kasus

Melanjutkan Tugas 1: FoodGo butuh sistem yang **decoupled** agar tim kurir dan tim resto tidak saling mengganggu ketika salah satu modul diperbarui/deploy ulang. Saat ini semua modul (pesanan, pembayaran, notifikasi kurir, katalog resto) berjalan sebagai satu aplikasi monolitik — sekali deploy, semua modul ikut restart dan berisiko downtime total.

## Tugas Kelompok

1. Pilih **satu** gaya arsitektur utama: **Service-Oriented Architecture (SOA)** atau **Publish-Subscribe**. Boleh dikombinasikan (mis. SOA untuk service inti + Pub-Sub untuk notifikasi), tapi harus dijustifikasi kenapa kombinasi ini yang dipilih.
2. Gambarkan minimal 4 komponen berikut dan interaksinya: modul Pesanan, modul Pembayaran, modul Kurir/Notifikasi, modul Katalog Resto (dan message broker/API gateway jika relevan).
3. Jelaskan alur satu skenario penuh secara end-to-end di diagram (misalnya: pelanggan buat pesanan → bayar → resto terima notifikasi → kurir ditugaskan) — tunjukkan komponen mana berkomunikasi dengan siapa, dan **jenis komunikasinya** (sinkron/asinkron, request-response/event).
4. Analisis tertulis: kenapa gaya ini mengatasi masalah *coupling* dari Tugas 1, dan apa trade-off-nya (mis. Pub-Sub menambah kompleksitas debugging karena alur tidak linear).

## Cara Membuat Diagram (Gratis, Cukup Laptop)

Tidak perlu software berbayar. Dua opsi:

**Opsi A — Mermaid di dalam Markdown (disarankan).** Ditulis sebagai teks biasa di `README.md`, otomatis dirender jadi diagram oleh GitHub — tidak perlu install apa pun.

````markdown
```mermaid
graph LR
  Client[Pelanggan] -->|HTTP request pesan| OrderSvc[Service Pesanan]
  OrderSvc -->|RPC sinkron| PaymentSvc[Service Pembayaran]
  OrderSvc -->|publish event OrderCreated| Broker[(Message Broker)]
  Broker -->|subscribe| NotifSvc[Service Notifikasi Kurir]
  Broker -->|subscribe| RestoSvc[Service Katalog Resto]
```
````

**Opsi B — draw.io / diagrams.net** (gratis, jalan di browser tanpa akun, atau app desktop offline di [app.diagrams.net](https://app.diagrams.net/)). Ekspor sebagai `.png` dan simpan di folder `diagram/`.

## Struktur Submission

```
tugas-02-perancangan-arsitektur/
├── README.md          # Analisis + diagram Mermaid (jika Opsi A) atau referensi ke diagram/
├── JURNAL.md
└── diagram/            # File .png/.drawio jika pakai Opsi B
```

## Rubrik Penilaian (Tugas 2)

| Komponen | Bobot | Kriteria |
|---|---|---|
| Ketepatan pemilihan gaya arsitektur | 20% | Justifikasi SOA/Pub-Sub sesuai kebutuhan *decoupling* di skenario |
| Kelengkapan & kejelasan diagram | 30% | Semua komponen kunci ada, jenis komunikasi (sinkron/asinkron) jelas ditandai |
| Analisis trade-off | 30% | Bukan hanya kelebihan — kekurangan/kompleksitas baru juga dibahas |
| Proses & kontribusi kelompok | 20% | `JURNAL.md`, commit history |

## Batasan Penggunaan AI (Level 2)

Kebijakan **Level 2 (AI Assisted Idea Generation & Structuring)** berlaku — lihat [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md). Boleh memakai AI untuk brainstorming komponen apa saja yang umum ada di gaya arsitektur SOA/Pub-Sub; **tidak boleh** meminta AI menggambar diagram final atau menuliskan analisis trade-off yang tinggal ditempel. Catat pemakaian AI di "Log Penggunaan AI" pada `JURNAL.md`.

- Diagram Mermaid/draw.io yang "terlalu generik" (identik dengan contoh tutorial di internet tanpa penyesuaian ke kasus FoodGo) akan dinilai rendah pada komponen kelengkapan & kejelasan diagram.

## Hasil Pengerjaan Kelompok

# 1. Pemilihan Arsitektur

Kami memilih kombinasi SOA dan Publish-Subscribe. SOA memisahkan fungsi sistem menjadi beberapa layanan, sedangkan Publish-Subscribe memungkinkan komunikasi asinkron melalui Message Broker.

# 2. Diagram Arsitektur

graph LR
    C[Pelanggan]
    O[Service Pesanan]
    P[Service Pembayaran]
    B[(Message Broker)]
    R[Service Katalog Resto]
    A[Service Penerimaan Pesanan]
    K[Service Kurir / Notifikasi]

    C -->|HTTP request, sinkron| O

    O -->|Request pembayaran, sinkron| P
    P -->|Respons pembayaran, sinkron| O

    O <-->|Request data menu, sinkron| R

    O -->|Publish OrderPaid, asinkron| B
    B -->|Subscribe OrderPaid| A

    A -->|Publish RestaurantAccepted, asinkron| B
    B -->|Subscribe RestaurantAccepted| K

    K -->|Notifikasi penugasan kurir, asinkron| C

# 3. Alur Sistem End-to-End

1. Pelanggan membuat pesanan melalui Service Pesanan.
2. Service Pesanan meminta pembayaran ke Service Pembayaran.
3. Setelah pembayaran berhasil, Service Pesanan menerbitkan event `OrderPaid` ke Message Broker.
4. Service Penerimaan Pesanan menerima event dan meneruskan pesanan ke resto.
5. Setelah resto menyetujui pesanan, event `RestaurantAccepted` diterbitkan.
6. Service Kurir/Notifikasi menerima event, memproses penugasan kurir, lalu mengirim notifikasi kepada pelanggan.

# 4. Analisis Coupling dan Trade-off

Arsitektur ini mengurangi coupling dengan memisahkan fungsi sistem menjadi layanan yang dapat diperbarui secara terpisah. Message Broker memungkinkan layanan bertukar event tanpa harus saling terhubung langsung. Namun, penggunaan Message Broker menambah kompleksitas debugging dan pemantauan. Sistem juga perlu menangani keterlambatan atau kegagalan pengiriman event serta kemungkinan pemrosesan event berulang.
