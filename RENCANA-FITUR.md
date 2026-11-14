# SkillSwap — Rencana Fitur

Prinsip: P0 = wajib ada supaya demo ujung-ke-ujung jalan. P1 = melengkapi cerita. P2 = pertama dicoret bila waktu mepet. AI saran skill = P2, dicoret pertama.

Perkiraan waktu di bawah ini kasar, dihitung sesudah FreshTrack selesai: P0 ~7–9 hari, P1 ~3–4 hari, P2 bila sempat.

## P0 — MVP (Phase 1–2, wajib)

1. **Profil skill** — tawar (teach) dan cari (learn): nama skill + tag, level, slot jadwal, bahasa.
2. **Daftar + detail + filter** — cari dan filter per kategori/level/bahasa.
3. **Matching engine v1** — skor 0–100 (skill 40, jadwal 30, reputasi 20, respons 10), ambang tampil ≥ 50, alasan skor ditampilkan.
4. **Pengajuan sesi** — ajukan, terima/tolak, jadwal disepakati.
5. **Escrow kredit waktu** — 1 jam = 1 kredit; ditahan saat sesi dijadwalkan, cair saat kedua pihak konfirmasi; sengketa menahan kredit.
6. **Ledger double-entry + audit log** — tiap pergerakan kredit tercatat dua sisi; ledger harus selalu seimbang.
7. **Konfirmasi dua pihak + arsip sesi** — sesi tidak bisa selesai dengan satu konfirmasi saja.
8. **Data seed** — 12 skill + 6 sesi contoh, dilabeli data demo.

## P1 — Pelengkap (Phase 3)

9. **Ulasan + reputasi** — ulasan hanya dari sesi selesai; skor memengaruhi match berikutnya.
10. **Feed live** — tawaran, match, dan sesi baru muncul tanpa refresh (polling ≤ 5 dtk).
11. **Animasi** — transisi kartu, progress kredit, skeleton loading; hormati `prefers-reduced-motion`.
12. **Sync multi-perangkat** — KV, debounce 1,5 dtk, last-write-wins, cap 64 KB / 200 item (pola FreshTrack).
13. **PWA** — install + offline baca daftar.
14. **Pengingat sesi in-app** — badge + pengingat saat aplikasi dibuka (tanpa push server).
15. **Moderasi** — laporkan profil/skill palsu; skor kepercayaan; tahan otomatis di bawah ambang.
16. **AI Skill Finder** — cari skill sesuai kebutuhan dari GitHub + ClawHub API v1 + katalog skill Hermes; hasil hanya tautan + ringkasan, tidak pernah install otomatis; tautan audit keamanan ClawHub ditampilkan.

## P2 — Dicoret dulu bila mepet (Phase 4, opsional)

17. **AI saran skill** dari deskripsi (Verboo); fallback manual bila AI gagal.
18. **Statistik profil** — jam mengajar/belajar, grafik sederhana.
19. **Bahasa UI kedua** (Indonesia).
20. **Export riwayat** (CSV).

## Urutan build (ketergantungan)

Profil → match → sesi → escrow → reputasi → live/animasi → sync/PWA → polish.
Escrow tidak bisa dites tanpa sesi; reputasi tidak bisa tanpa sesi selesai; feed live butuh data dari match/sesi.

## Definisi selesai per fitur

Tiap fitur: requirement di `.kiro/specs/skillswap/requirements.md` + tes (unit/PBT/Playwright) + uji HP user untuk P0/P1. Fitur tanpa tes = belum selesai.

## Yang TIDAK dibangun (tetap)

Pembayaran uang asli, video call di dalam app, verifikasi identitas resmi (KTP), chat bebas.
