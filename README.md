# Knowledge Library

Website catatan belajar pribadi berbasis Quartz 5, dengan Markdown sebagai sumber konten.

## Menjalankan di lokal

Prasyarat: Node.js 22 atau lebih baru, npm 10.9.2 atau lebih baru, dan Git. Pemasangan awal diverifikasi dengan Node 24.

```sh
npm ci
npx quartz plugin install
npm run dev
```

Buka `http://localhost:8080`. Preview otomatis membangun ulang saat konten berubah. Gunakan `Ctrl+C` untuk berhenti; server preview hanya untuk pengembangan lokal.

Untuk membuat output statis:

```sh
npm run build
```

Hasilnya berada di `public/`, yang tidak masuk Git.

## Menulis

- Beranda: `content/index.md`.
- Catatan: berkas Markdown di `content/`.
- Template: salin `content/templates/catatan.md` ke lokasi catatan baru dan ubah metadata.
- Catatan hanya terbit jika `publish: true` dan tidak memiliki `draft: true`.
- Folder `content/templates/` diabaikan dalam build.
- Lampiran di `content/` dapat ikut terbit meskipun catatan yang memakainya disaring. Masukkan hanya lampiran yang memang boleh dipublikasikan.

Obsidian dapat membuka `content/` sebagai vault. Editor teks dan AI agent juga dapat mengedit berkas yang sama. Tinjau perubahan sebelum commit.

## Konfigurasi

`quartz.config.yaml` memakai template Obsidian, locale `id-ID`, analytics nonaktif, dan font lokal. `baseUrl` disetel ke `morpknight.github.io/knowledge-library`; sesuaikan jika domain atau lokasi hosting berubah.

Konfigurasi sumber dan layout lanjutan tersedia melalui `quartz.ts`. Lisensi kode Quartz tetap mengikuti `LICENSE.txt`; tentukan lisensi tulisan secara terpisah bila diperlukan.

## Git

Branch kerja lokal: `main`. Riwayat dasar berasal dari [Quartz upstream](https://github.com/jackyzha0/quartz), branch `v5`, commit `97a2d05` saat pemasangan. Remote `upstream` menunjuk ke proyek Quartz. Clone awal bersifat shallow; ambil riwayat tambahan jika dibutuhkan untuk pembaruan.

Remote `origin` menunjuk ke repository pribadi di GitHub. Tinjau perubahan sebelum menggunakan `quartz sync`: perintah tersebut dapat melakukan commit, pull, dan push.

```sh
git status
git diff
```

Untuk pembaruan Quartz, mulai dari working tree bersih dan baca [panduan upstream](https://quartz.jzhao.xyz/getting-started/upgrading). Simpan lockfile dependency. Workflow bawaan upstream dibatasi untuk repository Quartz dan terpisah dari workflow Pages proyek ini.

## Catatan proyek

Riset awal tersedia di `docs/quartz-research.md`. Folder `docs/` juga memuat dokumentasi upstream; website pribadi dibangun dari `content/`.

GitHub Actions membangun dan menerbitkan situs ke [GitHub Pages](https://morpknight.github.io/knowledge-library/) setiap kali ada push ke `main`. Workflow juga dapat dijalankan secara manual dari tab Actions.

## Verifikasi pemasangan — 3 November 2026

Build produksi berhasil. Preview browser memverifikasi beranda, tautan ke catatan, backlinks, serta hasil pencarian. Build dengan dua catatan sementara membuktikan bahwa draft dan catatan tanpa `publish: true` tidak masuk output maupun indeks. Template juga tidak masuk output.

Template Obsidian upstream memuat tema tambahan yang gagal dimuat dan referensi Excalidraw yang belum tersedia. Tema tambahan dinonaktifkan dan referensi Excalidraw dihapus dari konfigurasi aktif; tampilan standar Quartz digunakan.

Pembaruan dependency yang kompatibel sudah diterapkan. Audit npm terakhir menyisakan dua temuan moderate pada minimatch/serve-handler; perbaikan otomatis yang ditawarkan melibatkan perubahan lintas versi mayor. Belum diterapkan. Serve mode digunakan untuk preview pengembangan dan dihentikan setelah pemeriksaan.
