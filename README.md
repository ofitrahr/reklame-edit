# Editor Titik Reklame Koridor — Batam

WebGIS interaktif untuk plotting dan penyuntingan titik reklame koridor di Kota Batam.

## Fitur

- **Drag & drop** — geser titik langsung di peta
- **Tambah titik** — pilih jenis (Koridor / Persimpangan / Pedestrian) lalu klik di peta
- **Hapus titik** — klik ikon ✕ pada kartu di panel kiri
- **Penggaris multipoint** — ukur jarak berantai, lengkap dengan total kumulatif
- **Penanda ruas pendek** — ruas <1 km yang belum punya titik ditandai garis oranye putus-putus
- **Auto-save** — progres otomatis tersimpan di browser (localStorage)
- **Simpan/Muat file proyek** — untuk backup atau pindah perangkat
- **Export** — GeoJSON dan CSV, langsung bisa dibuka di QGIS

## Cara Deploy ke GitHub Pages

1. Buat repository baru di GitHub (boleh publik atau privat)
2. Upload file `index.html` dan `README.md` ke repository tersebut
3. Masuk ke **Settings → Pages**
4. Pada bagian **Source**, pilih branch `main` dan folder `/ (root)`
5. Klik **Save**, lalu tunggu 1–2 menit
6. Situs akan tersedia di: `https://<username>.github.io/<nama-repo>/`

## Catatan Penting

- Auto-save tersimpan **per browser dan per perangkat**. Jika berganti komputer atau membersihkan cache, progres akan hilang.
- Gunakan tombol **💾 Simpan File** secara berkala sebagai cadangan.
- File hasil **Simpan File** (`proyek_reklame.json`) dapat dimuat kembali kapan saja melalui tombol **📂 Muat File**.

## Sumber Data

- Jalur potensial: `Jalur_Potensial_(Kolektor & Lokal).gpkg`
- Referensi lebar perkerasan (ROW): `Jalan_Kota_Batam_2024.gpkg` — kolom `Lbr_Keras`
- Batas administrasi: `BATAS_ADMINISTRASI_AR`

## Ketentuan Plotting

| Parameter | Nilai |
|---|---|
| Buffer dari ujung ruas | 500 m |
| Jarak antar titik | 500 m |
| Offset dari garis jalan | `Lbr_Keras / 2` |
| Pola penempatan | Zigzag kiri–kanan bergantian |

Hasil awal: **35 titik** dari 17 ruas (dari total 34 ruas).
