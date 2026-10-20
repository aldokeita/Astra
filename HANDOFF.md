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

- **Hitam** sebagai warna dasar, **galaksi putih bersinar** sebagai aksen.
- Konsep ikon: galaxy spiral putih yang bersinar di atas hitam.
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
  - ✅ **Keputusan basis branch (2026-10-20):** branch `astra` sudah di-rebase ke
    tag rilis `v2026.914.233613` (HEAD `c6da588a`, commit HANDOFF dipindahkan ke
    atas tag). Strategi patch-per-rilis resmi dipakai.
- **Fase 2 — Patch rebrand web UI & aset: SELESAI (2026-10-20)**
  1. ✅ Tema `astra` (blok `[data-theme="astra"]` + entri registry `theme.js`) dan
     ikon galaxy spiral putih bersinar untuk semua aset logo/status/tray.
     Ikon brand memakai set milik pemilik dari
     `/home/hydekeita/Documents/SoLunar/Astra Icon/Astra Icon.png`; tray/status
     memakai PNG (bukan SVG) agar detail galaxy tetap utuh.
     Tema `astra` kini default di browser bersih; pilihan `auto` tetap ada.
  2. ✅ Judul tab + `theme-color` + favicon (`template_header.html`), logo
     `Navbar.vue`/`NavbarSimple.vue`, username default `welcome.html` → `astra`.
  3. ✅ String `en.json` (70 nilai) di-rebrand ke Astra, ditambah kunci
     `navbar.theme_astra`. File `id.json` baru: 328 kunci mencakup seluruh UI
     utama (navbar, index, welcome, password, pin, apps, config penting, dsb);
     kunci teknis yang belum diterjemahkan jatuh ke fallback Inggris.
     `locale=id` didaftarkan di `config.cpp`, `General.vue`, dan
     `docs/configuration.md`.
  4. ✅ Deskripsi `CMakeLists.txt` (DESCRIPTION/BRIEF/LONG, homepage fork).
     `PROJECT_NAME`, `PROJECT_FQDN`, nama binary/paket/unit service/udev tetap.
     Ditambah variabel `PROJECT_DISPLAY_NAME="Astra"` untuk Name pada
     .desktop dan metainfo.
  5. ✅ Ikon app bawaan memakai set milik pemilik: `Desktop Icon.png` dan
     `Steam Icon.png` dari folder `Astra Icon`. Gaya hitam-putih menyala ini
     sengaja berbeda dari ikon berwarna Sunshine. Ikon `Epic Icon.png`,
     `Game Icon.png`, dan `Playnite Icon.png` sudah disalin ke
     `src_assets/common/assets/` untuk app tambahan di masa depan, tetapi belum
     didaftarkan sebagai app bawaan. **Nama app dipertahankan persis**
     ("Desktop", "Low Res Desktop", "Steam Big Picture") karena SoLunar
     mencocokkan "Desktop" persis dan mengenali "Steam"; ganti nama akan
     mematikan tombol "Hubungkan ke Desktop" di SoLunar.
  6. ✅ Ikon root (`sunshine.svg/png/ico`), ikon status playing/pausing/locked,
     README Astra yang bersih, dan URL update-check web UI diarahkan ke
     `aldokeita/Astra`.
  7. ✅ `packaging/linux/Arch/PKGBUILD.astra` (pkgname tetap `sunshine`,
     source branch `astra`, publisher aldokeita).
  - Validasi: build web UI (`npm ci && npm run build`) lolos; konsistensi
    locale (config.cpp ↔ General.vue ↔ file JSON) dicek ulang dan cocok.
  - ⚠️ **Catatan locale:** `id.json` belum 100% (353 kunci teknis masih
    Inggris lewat fallback). Lanjutkan saat menyentuh tab konfigurasi terkait.
- **Fase 3 — Build & tes otomatis: SELESAI (2026-10-20); validasi manual tersisa**
  - Build native `cmake-build-astra` dengan CMake 4.4.4 + GCC 16.2.1,
    `BUILD_TESTS=ON`, `BUILD_WERROR=ON`, CUDA OFF: **berhasil** (binary
    `sunshine` + `tests/test_sunshine`).
  - `test_sunshine`: **587 lulus, 6 skip, 0 gagal** saat
    `SystemTrayVisualTest.*` dikecualikan. Test visual itu butuh screenshot
    root/pointer warp yang tidak didukung niri; fixture ikon tray sudah
    diperbaiki agar menyalin PNG (sebelumnya hanya SVG).
  - Uji binary hasil build memakai state/port terpisah (`XDG_CONFIG_HOME` +
    `port=49089`, dibungkus `bwrap` untuk aset build): setup pertama lewat
    `/welcome` + `POST /api/password` berhasil, login Basic Auth aktif,
    locale `id` terpakai, dan browser Chrome bersih membuka beranda dengan
    `data-theme="astra"` serta judul **Astra**.
  - Uji kompatibilitas memakai **salinan** state/config `sunshine-vdd`
    (port 49199, path sendiri): `root.uniqueid` dan pairing lama `roth`
    terbaca identik; hash `sunshine_state.json` tidak berubah. Kedua instance
    produksi (`sunshine-xiaomi` port 48089, `sunshine-vdd` port 48119) tidak
    di-restart dan state aslinya tidak disentuh.
  - **Belum diverifikasi (butuh pemilik/SoLunar):** kriteria 1, 2, 5, 6, 7 di
    bawah. Terutama pairing baru harus diuji di instance uji terpisah dan
    rollback paket `sunshine` lama harus dicoba tanpa menghapus config/state.

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
