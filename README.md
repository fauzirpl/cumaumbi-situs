# Situs Cuma Umbi

Situs resmi game Cuma Umbi buatan CloudID Studio: <https://cumaumbi.cloudidstudio.biz.id>

Isinya cuma satu halaman depan (dua bahasa) dan kebijakan privasi, semuanya statis di `public/`.

## Jangan sunting `public/` dengan tangan

Isi `public/` dibangun dari repo game (`pns-simulator`) dan akan tertimpa saat dibangun ulang:

```bash
python tools/marketing/situs.py
```

Jalankan dari repo game. Skripnya mencari repo ini di sebelah repo game (`../cumaumbi-situs`); kalau letaknya lain,
set env `CUMAUMBI_SITUS` ke folder repo ini.

- Teks game (deskripsi, fakta, keterangan screenshot) ada di `tools/marketing/presskit.py`.
- Teks kebijakan privasi ada di `tools/marketing/privasi.py`. Klaimnya (tanpa izin Android, tanpa pencadangan
  data) dijaga tes di repo game, jadi ubah teksnya di sana dulu kalau perilaku game berubah.
- Gambar dibuat dari screenshot, logo, dan key art yang ada di checkout repo game.

Sesudah membangun ulang, commit repo ini lalu tarik di VPS.

**Alamat kebijakan privasi ini yang diisi di Play Console, jadi jangan dipindah:**
<https://cumaumbi.cloudidstudio.biz.id/privasi.html>

## Pasang di VPS

1. DNS domain `cloudidstudio.biz.id` ada di Sumopod. Tambahkan A record `cumaumbi` ke IP VPS.
2. Clone repo ini (`git clone https://github.com/fauzirpl/cumaumbi-situs.git /srv/cumaumbi-situs`), lalu
   sajikan `public/` dengan nginx:

   ```nginx
   server {
       listen 80;
       server_name cumaumbi.cloudidstudio.biz.id;
       root /srv/cumaumbi-situs/public;
       index index.html;
   }
   ```

3. HTTPS: `certbot --nginx -d cumaumbi.cloudidstudio.biz.id`
4. Memperbarui situs: `git pull` di folder repo. Tidak ada build di server.

Situs ini tidak memanggil server lain (huruf sistem, tanpa Google Fonts, analitik, atau cookie). Pilihan bahasa cuma
disimpan di localStorage peramban pengunjung.

## Pratinjau lokal

```bash
python -m http.server 8767 --bind 127.0.0.1 --directory public
```
