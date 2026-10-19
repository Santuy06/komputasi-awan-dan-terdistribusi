# Jurnal Proses — Tugas 5

## Implementasi Bully
- Kendala saat mengisi TODO 1-4: Kendala utama ada pada pemahaman alur komunikasi antar-node, terutama saat menerima pesan ELECTION, OK, dan COORDINATOR. Selain itu, perlu memahami cara mendeteksi leader yang mati dan memulai election kembali.

## Skenario 1: Start normal
- Leader yang terpilih: Node 5, sesuai aturan Bully Algorithm karena memiliki ID tertinggi.
- Log/observasi: Node dengan ID lebih rendah mengirim ELECTION ke node yang memiliki ID lebih tinggi. Node yang masih aktif membalas OK. Node 5 kemudian menjadi leader dan mengirim COORDINATOR ke node lainnya.

## Skenario 2: Leader dimatikan
- Leader baru yang terpilih: Node 4, sesuai aturan Bully Algorithm karena menjadi node dengan ID tertinggi yang masih aktif.
- Berapa lama waktu sampai leader baru terdeteksi: Sekitar beberapa detik setelah Node 5 dimatikan, bergantung pada interval pemeriksaan leader dan waktu tunggu election.

## Log Penggunaan AI (Level 2)

> Wajib diisi sesuai kebijakan Level 2 di [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md). Tulis "Tidak memakai AI" pada baris pertama jika memang tidak dipakai. Hanya untuk brainstorming ide/outline — bukan untuk kode/analisis/teks akhir.

| Tanggal | Tool AI | Prompt yang diberikan | Ringkasan saran/ide AI | Bagaimana diolah jadi tulisan/kode sendiri |
|---|---|---|---|---|
| 19 Oktober 2026 | ChatGPT | Meminta penjelasan implementasi Bully Algorithm pada simulasi lima node Python menggunakan TCP socket. | Mendapat penjelasan tentang penanganan ELECTION, OK, COORDINATOR, pemilihan leader, dan pengujian ketika leader mati. | Menggunakan saran sebagai referensi untuk memahami alur algoritma, mempelajari kode, dan melakukan pengujian sendiri. |