# Knowledge Library

Website catatan belajar pribadi berbasis Quartz 5, dengan Markdown sebagai sumber konten. Judul dan nama repository masih sementara.

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

`quartz.config.yaml` memakai template Obsidian, locale `id-ID`, analytics nonaktif, dan font lokal. `baseUrl` sementara adalah `localhost:8080`; ganti dengan domain/subpath hosting sebenarnya sebelum deployment.

Konfigurasi sumber dan layout lanjutan tersedia melalui `quartz.ts`. Lisensi kode Quartz tetap mengikuti `LICENSE.txt`; tentukan lisensi tulisan secara terpisah bila diperlukan.

## Git

Branch kerja lokal: `main`. Riwayat dasar berasal dari [Quartz upstream](https://github.com/jackyzha0/quartz), branch `v5`, commit `97a2d05` saat pemasangan. Remote `upstream` menunjuk ke proyek Quartz. Clone awal bersifat shallow; ambil riwayat tambahan jika dibutuhkan untuk pembaruan.

Remote `origin` dapat ditambahkan setelah repository GitHub dipilih. Jangan memakai `quartz sync` sebelum `origin` dikonfigurasi dan perubahan ditinjau: perintah tersebut dapat melakukan commit, pull, dan push.

```sh
git status
git diff
```

Untuk pembaruan Quartz, mulai dari working tree bersih dan baca [panduan upstream](https://quartz.jzhao.xyz/getting-started/upgrading). Simpan lockfile dependency. Workflow bawaan upstream masih mengikuti konfigurasi proyek Quartz; deployment website pribadi belum disiapkan.

## Catatan proyek

Riset awal tersedia di `docs/quartz-research.md`. Folder `docs/` juga memuat dokumentasi upstream; website pribadi dibangun dari `content/`.

Pilihan hosting yang direncanakan: GitHub Pages, homelab, atau Cloudflare Workers Static Assets. Belum ada deployment publik.

## Verifikasi pemasangan — 3 November 2026

Build produksi berhasil. Preview browser memverifikasi beranda, tautan ke catatan, backlinks, serta hasil pencarian. Build dengan dua catatan sementara membuktikan bahwa draft dan catatan tanpa `publish: true` tidak masuk output maupun indeks. Template juga tidak masuk output.

Template Obsidian upstream memuat tema tambahan yang gagal dimuat dan referensi Excalidraw yang belum tersedia. Tema tambahan dinonaktifkan dan referensi Excalidraw dihapus dari konfigurasi aktif; tampilan standar Quartz digunakan.

Pembaruan dependency yang kompatibel sudah diterapkan. Audit npm terakhir menyisakan dua temuan moderate pada minimatch/serve-handler; perbaikan otomatis yang ditawarkan melibatkan perubahan lintas versi mayor. Belum diterapkan. Serve mode digunakan untuk preview pengembangan dan dihentikan setelah pemeriksaan.
