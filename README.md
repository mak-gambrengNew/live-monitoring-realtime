# Monitoring Gerai — Teh Solo Ma-Gambreng

Production static PWA untuk halaman Monitoring operasional Gerai.

## Akses
- Halaman ini **publik** dan tidak memiliki halaman login.
- Siapa pun yang memiliki URL dapat melihat data Monitoring.
- Karena bersifat publik, data penjualan dan operasional yang ditampilkan di halaman ini juga dianggap data publik.

## Update data
- **Supabase Realtime** adalah jalur utama pembaruan Monitoring.
- Monitoring berlangganan perubahan operasional pada `monitoring_sales_events`, `monitoring_store_sales`, `monitoring_menu_sales`, dan `store_operation_sessions`.
- **Fallback otomatis setiap 5 menit (300 detik)** berjalan selama halaman terlihat dan mengambil ulang `monitoring-feed`.
- Saat koneksi kembali online atau halaman kembali terlihat, Monitoring melakukan pemeriksaan pembaruan.
- Waktu `Terakhir diperbarui` hanya berubah setelah feed berhasil diterima.
- Jika Realtime terputus, halaman tetap menampilkan data terakhir dan fallback tetap dapat menyegarkan data.

## UI flow
- Dashboard utama menampilkan ringkasan dan kartu Gerai yang ringkas.
- Tap Gerai untuk melihat detail penjualan, jumlah transaksi, jam mulai/tutup, dan menu terjual.
- Monitoring aktif ketika minimal satu Gerai sedang buka.
- Jika semua Gerai tutup, halaman masuk mode standby dan menampilkan pesan bahwa Monitoring live sedang tidak aktif.
- Saat Gerai kembali buka, dashboard aktif kembali otomatis.
- Tidak ada menu history/archive pada halaman Monitoring.

## Data architecture
PWA Gerai menulis data operasional ke Supabase.
Monitoring menjadi pembaca realtime khusus untuk operasional dan pembawa beban realtime tersebut.
Owner tidak melakukan subscription langsung ke tabel realtime operasional.

`monitoring-feed` menyediakan tampilan data teragregasi untuk halaman Monitoring.

## Deployment
- `index.html`
- `manifest.webmanifest`
- `sw.js`
- `vercel.json`

Deploy seluruh folder `monitoring-production` sebagai satu repo/static project.
