# Monitoring Gerai — Teh Solo Ma-Gambreng

Production static PWA untuk halaman Monitoring operasional Gerai.

## Akses
- Halaman ini **publik** dan tidak memiliki halaman login.
- Siapa pun yang memiliki URL dapat melihat data Monitoring.
- Karena bersifat publik, data penjualan dan operasional yang ditampilkan di halaman ini juga dianggap data publik.

## Update data
- **Supabase Realtime** adalah jalur utama pembaruan.
- Perubahan transaksi, penjualan menu, penjualan Gerai, dan sesi buka/tutup memicu sinkronisasi tampilan.
- **Fallback otomatis setiap 5 menit (300 detik)** mengambil ulang `monitoring-feed` untuk memastikan tampilan tetap sinkron.
- Waktu `Terakhir diperbarui` berasal dari keberhasilan menerima data terbaru dan ditampilkan dalam format `HH:MM:SS WIB`.
- Jika Realtime terputus, halaman tetap menampilkan data terakhir dan mencoba sinkronisasi melalui fallback.

## UI flow
- Dashboard utama menampilkan ringkasan dan kartu Gerai yang ringkas.
- Tap Gerai untuk melihat detail penjualan, jumlah transaksi, jam mulai/tutup, dan menu terjual.
- Monitoring aktif ketika minimal satu Gerai sedang buka.
- Jika semua Gerai tutup, halaman masuk mode standby dan menampilkan ringkasan hari sebelumnya.
- Saat Gerai kembali buka, dashboard aktif kembali otomatis.

## Data architecture
PWA Gerai menulis data operasional ke Supabase. Monitoring menjadi pembaca realtime khusus untuk operasional. Owner tidak melakukan subscription langsung ke tabel realtime operasional.

`monitoring-feed` menyediakan tampilan data teragregasi untuk halaman Monitoring.
