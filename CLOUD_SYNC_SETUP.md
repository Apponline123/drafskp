# Akun lintas perangkat

Cloudflare D1 dan Worker untuk aplikasi ini sudah dibuat:

- Database D1: `skp-cloud-sync`
- Worker URL: `https://notulen-ai-summary.dellano2098.workers.dev`
- Origin situs: `https://apponline123.github.io`

Tabel database sudah dibuat dan Worker sudah dihubungkan ke database. Aplikasi mengisi URL Worker secara otomatis; pengguna tidak perlu mengetik pengaturan teknis. Saat memperbarui atau menyalin aplikasi ke GitHub Pages, pertahankan konstanta `DEFAULT_CLOUD_API_URL` di `index.html`. Untuk aplikasi pada repositori `drafskp`, alamat situsnya `https://apponline123.github.io/drafskp/`.

## Memindahkan riwayat yang sudah ada tanpa menghapus data lokal

1. Buka situs aplikasi di perangkat/browser yang menyimpan kegiatan SKP atau riwayat notulen lama.
2. Masuk dengan akun lokal yang sama seperti sebelumnya. Jika akun tersebut belum ada di cloud, aplikasi akan memindahkan profil, kegiatan, dan notulen ke cloud setelah kata sandi lama diverifikasi.
3. Jika nama pengguna itu sudah memiliki akun cloud, masuk dengan kata sandi cloud. Perubahan lokal yang belum tersinkron akan digabungkan dengan data cloud.
4. Setelah status aplikasi menyatakan data tersinkron, buka situs di HP dan masuk dengan nama pengguna dan kata sandi cloud yang sama.
5. Simpan cadangan melalui menu cadangan sebelum mengganti atau menghapus data browser. Jangan menghapus data situs/browser selama pemindahan berlangsung.

Data pada perangkat lama tidak dihapus oleh migrasi. Data lokal dan catatan penghapusan ikut digabung; proses sinkronisasi gagal akan meninggalkan data di browser dan menampilkan status gagal.

## Menjalankan ulang migrasi atau memperbarui Worker

Database ID untuk deployment CLI tersimpan di `wrangler.toml`. Pastikan Wrangler terhubung ke akun Cloudflare yang benar, lalu gunakan:

```powershell
npx wrangler d1 execute skp-cloud-sync --remote --file=schema.sql
npx wrangler deploy
```

`ALLOWED_ORIGIN` harus sama persis dengan origin situs (`https://apponline123.github.io`), bukan URL Worker. URL Worker yang dipakai pengguna adalah alamat `workers.dev` di atas.

## Catatan penyimpanan

- Riwayat browser lama hanya bisa dipindahkan dari perangkat/browser yang menyimpan riwayat itu; masuk di HP baru tidak dapat membaca penyimpanan browser di perangkat lain.
- Maksimum satu permintaan sinkronisasi adalah 1,8 MB. Jika melebihi batas, ekspor cadangan dan kurangi ukuran foto.
- Data profil pegawai, SKP, dan notulen disimpan di akun Cloudflare ini. Kata sandi di-hash dengan PBKDF2, tidak disimpan sebagai teks biasa.
- Jangan membuka `index.html` langsung dengan `file://` untuk sinkronisasi; gunakan situs GitHub Pages melalui HTTPS.
