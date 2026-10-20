# Astra — HANDOFF

Dokumen serah-terima proyek Astra: status, keputusan, dan rencana kerja.
Diperbarui terakhir: **2026-10-20**.

## Apa itu Astra

Astra adalah fork dari [LizardByte/Sunshine](https://github.com/LizardByte/Sunshine)
(GPLv3) yang di-rebrand menjadi platform host milik Aldo untuk aplikasi-aplikasi
jembatan PC ↔ Android/iOS buatannya. Client pertamanya adalah
[SoLunar](https://github.com/aldokeita/SoLunar) (fork Moonlight Android);
client lain direncanakan menyusul. Rebrand ini bersifat dangkal (shallow):
identitas dan tampilan diganti, mesin streaming upstream dipertahankan apa adanya.

- Repo: `aldokeita/Astra` (fork dari `LizardByte/Sunshine`, dibuat 2026-10-20)
- Branch kerja: `astra` — semua commit rebrand masuk ke sini, JANGAN ke `master`
- Platform sasaran sekarang: **Arch/CachyOS dan Linux umumnya**; Windows masuk daftar publish
- Build & tes: di mesin CachyOS pemilik (toolchain lokal), bukan di CI untuk saat ini

## Keputusan brief (2026-10-20, dari pemilik)

1. Nama produk: **Astra** — platform host umum, bukan hanya pasangan SoLunar.
2. `sunshine_name` per instance **tetap deskriptif per monitor** (tidak diganti).
3. Cakupan rebrand: seluruh titik hasil audit (lihat "Peta titik branding").
4. Tema web UI `astra` menjadi **tema default**.
5. Nama binary & nama paket **tetap `sunshine`** untuk sekarang.
   ⚠️ **Pengingat masa depan:** saat Astra mendekati publish, ingatkan pemilik soal
   mengganti nama binary — keputusan ini sengaja ditunda, jangan dilakukan diam-diam.
6. Strategi branch: patch rebrand hidup di branch `astra`, di-rebase ke tag rilis
   upstream tiap ada rilis baru (±bulanan).
7. String web UI **wajib dwibahasa**: Inggris default, Indonesia opsi kedua.
8. README fork bersih beridentitas Astra; atribusi upstream ditambahkan manual
   oleh pemilik. `LICENSE` dan `NOTICE` upstream tetap utuh.
9. App bawaan host (Desktop, Low Res Desktop, Steam Big Picture) mendapat
   **nama + ikon ala SoLunar**.

## Identitas visual

- **Hitam** sebagai warna dasar, **galaksi konstelasi putih** sebagai aksen.
- Konsep ikon: rasi bintang putih yang membentuk huruf "A" di atas hitam.
- Kontras identitas: SoLunar = gerhana + merah; Astra = langit malam + bintang putih.
- Draft awal (palet tema CSS + SVG ikon) disiapkan terpisah dan diterapkan pada Fase 2;
  variabel tema mengikuti struktur `[data-theme]` di `sunshine.css` upstream
  (latar `#050607`, permukaan `#0e1116`, aksen starlight `#c9d6ff`).

## Yang TIDAK boleh disentuh

- Protokol streaming & isi serverinfo selain `hostname` (`src/nvhttp.cpp`)
- Port bawaan (47989/47990/dst.) dan tipe layanan mDNS `_nvstream._tcp`
- Format config `sunshine.conf` dan nama file state `sunshine_state.json`
- Nama binary `sunshine`, nama unit service, aturan udev `60-sunshine.rules`

Alasan: fork yang membaca folder config yang sama (`~/.config/sunshine` dan
`~/.config/sunshine-xiaomi`) mewarisi pairing kedua instance yang sudah berjalan
di mesin pemilik beserta watchdog-nya — tidak ada pairing ulang.

## Peta titik branding (hasil audit 2026-10-20, ringkasan)

| Permukaan | Lokasi |
|---|---|
| Nama host di client | config `sunshine_name` (default hostname, `src/config.cpp`) — level config |
| Judul tab + favicon web UI | `src_assets/common/assets/web/index.html` |
| Logo navbar web UI | `web/Navbar.vue` + `web/public/images/logo-sunshine-{16,45}.png` |
| String UI | `web/public/assets/locale/en.json` (95 kata "Sunshine") + locale `id` |
| Username default setup | `web/Welcome.vue` (`newUsername: "sunshine"`) |
| Tema web UI | blok `[data-theme]` di `web/sunshine.css` + registry `web/theme.js` |
| Ikon status | `web/public/images/sunshine-{playing,pausing,locked}.*` |
| Deskripsi proyek | `CMakeLists.txt` (DESCRIPTION/BRIEF/LONG) — mengalir ke .desktop, metainfo, paket |
| Ikon OS | `sunshine.svg/png/ico` di root + `packaging/linux/*.desktop`, `*.metainfo.xml` |
| App bawaan | `src_assets/linux/assets/apps.json` + ikon di `src_assets/common/assets/` |
| Paket Arch | `packaging/linux/Arch/PKGBUILD` (fork perlu PKGBUILD sendiri) |

Perkiraan patch inti: ±15–25 file, hampir semua teks/aset.

## Status

- **Fase 1 — Fondasi repo: SELESAI (2026-10-20)**
  - Fork `aldokeita/Astra` terbuat dengan semua branch + tag upstream. ✅
  - Branch `astra` terbuat. ✅
  - ⚠️ **Catatan terbuka:** branch `astra` saat ini berbasis `master` upstream
    (HEAD `8aac196`), BUKAN tag `v2026.914.233613` seperti strategi awal —
    pemilih branch web GitHub tidak menawarkan tag dan API pembuatan branch
    sedang tidak stabil. Keputusan sebelum Fase 2 dimulai: rebase branch ke
    tag `v2026.914.233613`, atau lanjut dari master. Belum diputuskan.
  - Belum ada commit rebrand apa pun di branch `astra`.
- **Fase 2 — Patch rebrand web UI & aset: BELUM MULAI**
  Urutan kerja yang direncanakan:
  1. Tema `astra` (CSS + registry + jadikan default) dan ikon konstelasi.
  2. Judul/favicon `index.html`, logo `Navbar.vue`, username default `Welcome.vue`.
  3. String dwibahasa: patch `en.json`, lengkapi locale Indonesia untuk string yang diubah.
  4. Deskripsi `CMakeLists.txt` (deskripsi saja; `PROJECT_NAME` tidak diganti).
  5. `apps.json` bawaan: nama + ikon ala SoLunar.
  6. Ikon root repo + banner; README Astra.
  7. PKGBUILD Arch untuk fork.
- **Fase 3 — Build & tes di CachyOS pemilik: BELUM MULAI** (kriteria di bawah)

## Kriteria tes (disetujui pemilik 2026-10-20)

1. Dua instance nyala; pairing lama langsung terbaca; watchdog tidak terganggu.
2. SoLunar bisa stream dari kedua monitor seperti sebelumnya.
3. Web UI tampil berbrand Astra; tema `astra` aktif default di browser bersih.
4. Setup pertama dari nol jalan (username default baru + ganti password).
5. Pairing baru dites di instance uji terpisah (port & state sendiri), bukan di produksi.
6. Fitur client yang membaca host tetap akurat: tes jaringan, lembar detail, ganti monitor.
7. Rollback aman: paket `sunshine` lama bisa dipasang balik tanpa kehilangan pairing/config.

## Konvensi kerja

- Semua pekerjaan rebrand di branch `astra`; `master` dibiarkan identik upstream
  agar sinkronisasi berkala gampang.
- Aset biner (PNG/ICO) tidak bisa didorong lewat API konektor GitHub (khusus teks
  UTF-8) — gunakan jalur web GitHub untuk file biner; SVG/teks bisa lewat API.
- Dokumen pendamping: audit lengkap titik branding & brief final disimpan pemilik
  proyek di luar repo (workspace Muse: `sunshine-audit/AUDIT.md`, `BRIEF.md`).
