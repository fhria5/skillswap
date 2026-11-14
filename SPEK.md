# SkillSwap — Spek

**Nama:** SkillSwap — Trade Skills with Time Credits
**Kategori Kiroween:** Frankenstein ("Stitch together a chimera of technologies into one app" — kiroween.devpost.com/rules, terverifikasi 14 Nov 2026)
**Repo:** https://github.com/fhria5/skillswap (publik, MIT, `/.kiro` di root)
**Status:** TAHAP SPEK SAJA (14 Nov 2026). Belum ada satu baris kode aplikasi pun. Build dimulai HANYA setelah FreshTrack dinyatakan sempurna oleh user.

## Masalah

Skill orang menganggur, belajar skill baru mahal. Platform kursus satu arah: bayar, nonton, selesai. Tidak ada cara yang adil untuk tukar skill antar orang — saya ajari kamu gitar, kamu ajari saya bahasa Inggris — tanpa uang.

## Pengguna

Siapa saja yang punya satu skill dan mau belajar skill lain; pelajar, freelancer, komunitas.

## Alur inti (satu alur)

Tawar skill (apa yang bisa diajarkan + apa yang mau dipelajari) → matching engine (skill cocok + jadwal cocok + reputasi) → ajukan sesi → sesi berjalan → kedua pihak konfirmasi → kredit waktu cair (escrow) → ulasan + reputasi naik.

## Fitur

1. Profil skill: tawar (teach) dan cari (learn), level, jadwal tersedia, bahasa.
2. Matching engine: skor 0–100 per pasangan (skill 40, jadwal 30, reputasi 20, respons 10), ambang tampil ≥ 50, alasan skor ditampilkan.
3. Pengajuan sesi + jadwal: ajukan, terima/tolak, jadwal disepakati; pengingat.
4. Escrow kredit waktu: 1 jam sesi = 1 kredit; kredit pembelajar ditahan saat sesi disetujui, cair ke pengajar saat kedua pihak konfirmasi selesai. Sengketa → status disputed, kredit kembali ditahan.
5. Ulasan + reputasi: ulasan hanya dari sesi yang selesai; skor reputasi memengaruhi match berikutnya.
6. Feed live: tawaran, match, dan sesi baru muncul tanpa refresh (polling ≤ 5 dtk).
7. Animasi: transisi kartu, progress kredit, skeleton loading; hormati `prefers-reduced-motion`.
8. PWA: install, offline baca daftar.
9. AI Skill Finder: pengguna ketik kebutuhan ("saya mau belajar ...") → AI mencari skill dari GitHub, ClawHub, katalog skill Hermes, dan sumber lain → hasil: nama, deskripsi, sumber, tautan, skor kecocokan. Hasil hanya tautan + ringkasan — TIDAK pernah install otomatis; tiap hasil ClawHub menampilkan tautan audit keamanannya.

## Backend (kedalaman)

- Matching engine: skill graph (tag + level) + kecocokan jadwal (slot overlap) + reputasi + laju respons; bobot 40/30/20/10.
- Ledger kredit waktu (double-entry sederhana): setiap pergerakan kredit tercatat dua sisi (debit pembelajar, kredit pengajar); escrow state machine: offered → matched → scheduled → in_progress → confirmed → settled / disputed → resolved.
- Reputasi: skor dari ulasan sesi selesai saja (bukan ulasan bebas); laju respons dihitung dari riwayat.
- Sinkron multi-perangkat: pola FreshTrack — Workers KV, debounce 1,5 dtk, last-write-wins, cap 64 KB / 200 item.
- Audit log: semua transisi sesi + pergerakan kredit tercatat (hash-chain sederhana, pola SentinelBot).
- Search aggregator (AI Skill Finder): Worker memanggil GitHub Search API (tanpa auth: 10 req/menit, hasil di-cache 1 jam; token pengguna opsional di pengaturan lokal, tersimpan di perangkat saja) + ClawHub API v1 (GET /api/v1/search) + indeks katalog skill Hermes (daftar statis dari repo/folder yang ditunjuk pengguna); AI (Verboo) merangkum + mencocokkan kebutuhan ke hasil; timeout 15 dtk; bila satu sumber gagal, sumber lain tetap tampil + diberi label.
- Rate limit 30 req/IP/menit; CORS allowlist; validasi input (cap 2.000 karakter).
- AI (dua kegunaan): (a) AI Skill Finder (P1) — cari skill dari GitHub + ClawHub + katalog Hermes sesuai kebutuhan pengguna; (b) saran skill dari deskripsi (P2). Keduanya opsional, BUKAN syarat Kiroween. Bila waktu mepet, (b) dicoret dulu; (a) dipertahankan bila sempat, bila tidak sempat dicoret tanpa mengurangi keabsahan submit.

## Kenapa Frankenstein

Empat elemen yang tidak nyambung dijahit jadi satu app: marketplace skill + ledger kredit waktu (escrow) + sistem reputasi + penjadwalan. Tidak ada satu pun yang berdiri sendiri sebagai SkillSwap.

## Tidak statis (keputusan user 14 Nov ~16:56, "Keduanya")

- Live: feed tawaran/match/sesi baru muncul sendiri tanpa refresh.
- Animasi: UI bergerak (transisi, progress, skeleton), bukan halaman diam.

## Tech

- Frontend: PWA statis (HTML/CSS/JS vanilla, tanpa framework), GitHub Pages.
- Backend: Cloudflare Worker klasik di route `grokingstore.my.id/skillswap-api/*` (pola Gembala/FreshTrack — Worker baru TIDAK dapat rute workers.dev) + Workers KV `skillswap-ledger`.
- Bahasa UI: Inggris (audiens internasional).
- Kiro: spec-driven (`.kiro/specs/skillswap`), steering, hook check-js-on-save, MCP server read-only, PBT (pola FreshTrack Phase 26/28), agen QA.

## Aturan Kiroween yang mengikat

- Proyek baru (periode 30 Okt–4 Des 2026); repo publik + lisensi terdeteksi + `/.kiro` di root, JANGAN di-gitignore.
- Video demo < 3 menit, YouTube Public.
- Write-up pemakaian Kiro wajib jujur soal porsi yang dibangun Kiro vs tangan (pelajaran FreshTrack).
- Satu proyek = satu kategori; multi-submit boleh asal tiap proyek jauh berbeda (SkillSwap beda domain dan kategori dari FreshTrack/Costume Contest).
- Deadline submit: 5 Des 2026 06:00 WITA. Target selesai build: 30 Nov–2 Des.

## Cold start (data seed)

Demo dan video wajib pakai data seed: 12 skill contoh + 6 sesi contoh, supaya match dan feed kelihatan hidup sejak pertama dibuka.

## Non-goal (tidak dibangun)

Pembayaran uang asli (kredit hanya waktu, bukan uang), video call di dalam app (sesi pakai alat luar, yang dicatat hanya jadwal + konfirmasi), verifikasi identitas resmi (KTP).
