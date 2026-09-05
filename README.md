# Editor Titik Reklame Koridor — Batam

WebGIS interaktif untuk plotting dan penyuntingan titik reklame koridor di Kota Batam.

## Fitur

- **Drag & drop** — geser titik langsung di peta
- **Tambah titik** — pilih jenis (Koridor / Persimpangan / Pedestrian) lalu klik di peta
- **Hapus titik** — klik ikon ✕ pada kartu di panel kiri
- **Penggaris multipoint** — ukur jarak berantai, lengkap dengan total kumulatif
- **Patok jarak** — penanda tiap 100 m di sepanjang ruas, dihitung dari awal ruas
  (kerapatan menyesuaikan zoom: 100 m / 500 m / 1000 m; bisa dimatikan)
- **Koordinat siap salin** — klik koordinat di kartu, atau tombol 📋 Salin di popup,
  lalu tempel ke kotak pencarian Google Earth / Maps; tersedia juga tautan langsung
- **Penanda ruas pendek** — ruas <1 km yang belum punya titik ditandai garis oranye putus-putus
- **Titik jauh dari jalan** — 98 titik reklame yang >50 m dari ruas terdekat, ditampilkan
  sebagai belah ketupat merah agar bisa digeser ke jalan; lihat bagian di bawah
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

## Patok Jarak

Patok dihitung dengan interpolasi haversine sepanjang geometri tiap ruas, mulai dari
titik awal ruas (`0 m`). Total 433 patok. Yang dirender hanya patok di dalam viewport,
dan kerapatannya mengikuti zoom:

| Zoom | Patok tampil |
|---|---|
| >= 17 | tiap 100 m |
| >= 15 | tiap 500 m |
| >= 13 | tiap 1000 m |
| < 13 | sembunyi |

Patok bersifat non-interaktif (`pointer-events: none`), jadi tidak mengganggu mode
tambah titik, penggaris, maupun drag marker.

> Catatan: panjang hasil hitung haversine berbeda ~0,5% dari kolom `len` bawaan data
> (mis. 426 m vs 424 m) karena `len` dihitung di CRS terproyeksi.

## Titik Jauh dari Jalan

Tab **TITIK JAUH** berisi 98 titik dari `Titik Reklame Terbaru _ VER 20260905 13.30.gpkg`
yang jaraknya lebih dari 50 m dari ruas terdekat pada `Jalan Kota Batam 2024.gpkg`,
sehingga kolom `NAMA_JALAN` dan `ROW`-nya kosong setelah overlay.

Cara pakai: klik kartu untuk melompat ke titiknya, lalu geser belah ketupat merah ke
ruas jalan. Garis putus-putus menunjuk ke titik terdekat pada jalan dan jaraknya
dihitung ulang setiap kali digeser. Belah ketupat berubah hijau begitu jaraknya
<= 50 m. Ekspor GeoJSON/CSV memuat koordinat lama dan baru plus penanda `DIGESER`,
untuk di-join kembali ke GPKG lewat kolom `UID`.

Selama titik masih >50 m, **nama ruas sengaja dikosongkan** (`—` di kartu dan popup,
`JALAN_TERDEKAT` kosong di ekspor) dan baru terisi setelah titiknya digeser sampai
<= 50 m. Alasannya ada di catatan di bawah: ruas "terdekat" sejauh ratusan meter
belum tentu jalan tempat titik itu benar-benar berdiri, jadi menampilkannya justru
menyesatkan. Perilaku ini sejalan dengan GPKG, yang juga mengosongkan `NAMA_JALAN`
dan `ROW` untuk ke-98 titik tersebut.

Jaringan jalan yang ditampilkan hanya ruas dalam radius 600 m dari ke-98 titik
(66 ruas, disederhanakan ~0,5 m). Jarak dihitung dengan proyeksi lokal
equirectangular; selisihnya terhadap perhitungan UTM di QGIS sekitar 1 m,
dan selalu sedikit lebih besar, jadi aman terhadap ambang 50 m.

> **Penting:** `Jalan Kota Batam 2024.gpkg` adalah inventarisasi jalan resmi
> (1.184 ruas, 1.233 km) dan tidak memuat seluruh jalan yang ada di lapangan.
> Sebagian titik berdiri di jalan nyata yang memang tidak ada di layer itu —
> menggesernya ke ruas terdekat justru akan memindahkannya dari lokasi
> sebenarnya. Periksa citra satelit dulu sebelum menggeser.
