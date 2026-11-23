---
name: "ai_live_action_short_drama"
description: "Workflow membuat film pendek DRAMA LIVE-ACTION photorealistic dengan AI — adaptasi dari skill 'Animated Short Film' (Đặng Hữu Sơn / sonlovinbot-team, MIT) dengan SELURUH visual direction diganti dari animasi menjadi photorealistic live-action cinematic. Dipakai saat user ingin membuat short drama live-action: character reference sheets aktor asli, story bible & screenplay, cinematic keyframes, prompt first-frame/last-frame, video per adegan, voice-over & sound design, sampai MP4 final. DILARANG gaya animasi/kartun/anime/3D/Disney/Pixar dalam workflow ini."
---

# AI Live-Action Short Drama Pipeline

**Adaptasi dari:** "Animated Short Film" oleh Đặng Hữu Sơn (github.com/sonlovinbot-team/Muse-animated-short-film, lisensi MIT).
**Yang dipertahankan:** seluruh workflow 7 tahap + Fase 0 + aturan operasionalnya (aspect ratio lock, character lock,
continuity match-cut, realism law, verify-before-mix, normalize-before-concat, VO storytelling).
**Yang diganti total:** visual direction — dari animasi 2D menjadi **photorealistic live-action cinematic drama**.

---

## ⚠ GERBANG KEMAMPUAN (dibaca & dijalankan SEBELUM proyek dimulai)

Hasil tes nyata di environment ini, **2026-11-13**:

| Kemampuan | Status | Bukti |
|---|---|---|
| Image generation photorealistic live-action | ✅ **TERDUKUNG** | Tes still: aktor pria Indonesia ~30-an di jendela hujan — tekstur kulit & pori realistis, anatomi benar, cahaya sinematik natural, depth of field film. File: `~/workspace/live-action-drama/tests/` |
| Video generation (media.generate_video) | ❌ **TIDAK TERSEDIA** | Error resmi: "Video generation is not available for this account in its country or region" (403, batasan permanen region — retry tidak akan membantu) |

**Konsekuensi jujur:** Tahap 5 (generate video) TIDAK bisa dijalankan di environment ini.
JANGAN menggantinya diam-diam dengan hasil animasi/kartun — itu melanggar inti workflow ini.
Dua jalur resmi untuk Tahap 5 (user memilih di Fase 0):

- **Jalur A — Generate eksternal, rakit di sini (paling setia pada visi live-action bergerak):**
  Muse menyiapkan character sheets, keyframes, dan MEGA_PROMPTS.md yang copy-paste-ready (termasuk first-frame &
  last-frame images photorealistic). User menjalankan prompt di tool video live-action yang bisa dia akses sendiri
  (mis. Veo / Kling / Runway / Pika — di luar environment ini), lalu mengirim clip-clip hasilnya ke sini.
  Muse mengerjakan Tahap 6 (VO & sound design) dan Tahap 7 (editing MP4 final) dari clip tersebut.
- **Jalur B — Photoreal stills film (fallback, WAJIB dilabeli jujur):**
  Keyframe photorealistic digerakkan dengan gerakan kamera sinematik di Remotion/ffmpeg (slow push-in, pan, parallax
  antar-layer, film grain, letterbox bila 16:9) + VO + sound design penuh. Hasilnya MP4 drama berbasis stills —
  **bukan** video akting bergerak, dan harus disampaikan ke user apa adanya sebagai stills film, bukan live-action motion.

Jika suatu saat video generation tersedia di region ini, gerbang ini dites ulang (1 clip tes) dan Tahap 5
kembali ke jalur utama: generate per adegan langsung di sini, ±10 detik per clip.

---

## VISUAL DIRECTION LOCK (ditempel UTUH ke SETIAP prompt gambar & video proyek ini)

> **PHOTOREALISTIC LIVE-ACTION LOCK:** This is a photorealistic live-action cinematic drama film, shot with real
> human actors in real-world locations. Realistic human anatomy, natural skin texture with visible pores and
> imperfections, natural facial expressions and emotional acting, believable human interactions. Natural cinematic
> lighting, professional movie cinematography, realistic camera movement, shallow depth of field, subtle film grain,
> realistic color grading. **STRICTLY NOT animation: no cartoon, no anime, no 3D render, no CGI character look,
> no Disney style, no Pixar style, no illustration, no painterly or stylized look.** Every frame must be
> indistinguishable from a still of a professionally shot live-action drama film.

## CHARACTER IDENTITY LOCK (per karakter, ditulis sekali di Story Bible, disalin UTUH ke setiap prompt)

Setiap aktor dikunci dengan blok identitas tekstual yang TIDAK BOLEH berubah antar-adegan, contoh format:

> **ACTOR LOCK — [NAMA]:** a real [etnis] [man/woman], age [exact age], [height/build], [face shape], [skin tone and
> texture details], [hair: color, length, style — FIXED], [distinctive marks: mole/scar/stubble — FIXED].
> Wardrobe (fixed unless the story explicitly changes it): [itemized clothing]. Same face, same age, same hairstyle,
> same body proportions in EVERY scene. No beautifying, no de-aging, no face drift.

Aturan konsistensi:
1. Character reference sheet aktor adalah ref PERTAMA yang dimuat di setiap prompt generate (gambar maupun video).
2. Wajah, usia, rambut, pakaian, dan proporsi tubuh tidak berubah tanpa alasan yang tertulis di screenplay
   (mis. luka di adegan 5 → boleh muncul bekasnya di adegan 6+, dicatat di continuity log Story Bible).
3. Output yang wajahnya meleset (face drift), terlihat lebih muda/tua, atau bergaya kartun = **ditolak, generate ulang**.
   Jangan pernah "dimaklumi" ke tahap berikutnya.

## STRICT ASPECT RATIO LOCK (dari skill asli, dipertahankan utuh)

> **STRICT ASPECT RATIO LOCK:** The final output video MUST be **[16:9 landscape | 9:16 vertical — pilih 1]**.
> All reference images — character sheets, context sheets, keyframes, first-frame and last-frame images — MUST share
> this exact aspect ratio. Reference images are not only style and character guides; they directly condition the
> output framing and composition. Any reference image with a mismatched aspect ratio MUST be regenerated or cropped
> to the locked ratio before use. NEVER mix aspect ratios within a single project.

Verifikasi rasio dengan ffprobe di setiap tahap; salah rasio = perbaiki saat itu juga.

---

## Fase 0 — Pengumpulan Input (WAJIB, tanya sebelum mengerjakan apa pun)

1. **Durasi film yang diinginkan?** Setiap clip video ≈ 10 detik. **Jumlah adegan = ceil(durasi ÷ 10).**
   (60 dtk → 6 adegan; 80 dtk → 8 adegan; 45 dtk → 5 adegan.) Kunci angka ini — semua tahap bergantung padanya.
2. **Bahasa utama?** Menentukan bahasa VO, VO_SCRIPT.md, dan diskusi screenplay. (Default user: Bahasa Indonesia.)
3. **Rasio: 16:9 (landscape) atau 9:16 (vertikal)?** Mengunci SEMUA ref input (lihat Aspect Ratio Lock).
4. **Jalur Tahap 5: A (generate eksternal) atau B (stills film)?** — karena gerbang kemampuan di atas.
   Tanpa jawaban jalur, proyek berhenti di Tahap 4 (prompts siap) — itu titik henti yang sah.

Belum ada jawaban lengkap → belum masuk Tahap 1.

## Workflow — 7 Tahap (struktur asli, isi diganti live-action)

### Tahap 1 — Character Reference Sheets (AKTOR, bukan karakter kartun)
- Untuk setiap aktor: satu sheet photorealistic rasio terkunci berisi: potret wajah close-up (depan, 3/4, profil),
  full-body depan & samping, dan baris ekspresi wajah natural (netral, sedih, marah tertahan, tersenyum tipis, terkejut).
- Sheet digenerate dengan image generation + PHOTOREALISTIC LOCK + blok ACTOR LOCK aktor tsb.
- Sheet ini menjadi "kunci aktor" — dimuat pertama di semua prompt berikutnya. Wajah di sheet = wajah final;
  kalau user menolak wajahnya, sheet diganti SEKALI di tahap ini, bukan di tengah produksi.

### Tahap 2 — Story Bible & Screenplay
- Diskusi & kunci dengan user: logline, karakter, dunia/lokasi, konflik, akhir cerita.
- Tulis **STORY.md**: logline; ACTOR LOCK tiap aktor (utuh); PHOTOREALISTIC LOCK; aturan continuity;
  per adegan: beat cerita, lokasi & waktu (day/night), dialog/aksi aktor, deskripsi FIRST FRAME dan LAST FRAME,
  baris VO, musik/sound design. Jumlah adegan = angka Fase 0.
- Screenplay ditulis seperti naskah film drama beneran (INT./EXT., nama lokasi, aksi, dialog) — bukan sinopsis kartun.

### Tahap 3 — Cinematic Keyframes
- Generate K1..K(N+1) keyframe photorealistic rasio terkunci; setiap keyframe memuat character sheet aktor yang tampil.
- Keyframe terlihat seperti still film sungguhan: komposisi sinematik (rule of thirds, negative space, foreground depth),
  lighting termotivasi sumber nyata (jendela, lampu jalan, neon), bukan lighting studio kartun.
- **Match-cut law:** keyframe terakhir adegan N = keyframe pertama adegan N+1.

### Tahap 4 — Mega Prompt per Adegan (first-frame & last-frame)
Satu mega prompt copy-paste per adegan, struktur tetap:
1. PHOTOREALISTIC LIVE-ACTION LOCK (utuh) + STRICT ASPECT RATIO LOCK (utuh)
2. ACTOR LOCK semua aktor di adegan itu (utuh)
3. Urutan muat ref: character sheet → first-frame image → last-frame image → context lain
4. **FIRST FRAME:** deskripsi frame pembuka persis keyframe K(n)
5. **MOTION / ACTING:** gerakan & akting aktor yang natural dan terukur (mikro-ekspresi, bahasa tubuh realistis;
   satu aksi utama per clip ±10 detik — jangan menumpuk banyak aksi)
6. **CAMERA:** gerakan kamera film nyata (static tripod, slow dolly-in, handheld subtle, pan) + lensa/depth of field
7. **LAST FRAME:** deskripsi frame penutup persis keyframe K(n+1)
8. **SOUND / MUSIC / VO:** ambience lokasi, musik, dan baris VO adegan itu
- Di Jalur A file ini diserahkan ke user untuk dijalankan eksternal; di Jalur B, bagian MOTION diterjemahkan menjadi
  gerakan kamera Remotion atas still (push-in/pan/parallax) — akting bergerak tidak dijanjikan.

### Tahap 5 — Video per Adegan
- **Jalur utama (bila video generation tersedia):** generate per adegan dari mega prompt + ref; verify tiap clip:
  on-model (wajah = ACTOR LOCK), photorealistic (tidak ada bau kartun/3D), rasio & durasi via ffprobe. Gagal = ulang.
- **Jalur A:** user generate eksternal dari MEGA_PROMPTS.md; clip dikumpulkan ke `videos/`, diverifikasi sama
  (on-model, rasio, durasi, cek stream audio bawaan clip).
- **Jalur B:** render segmen stills-film per adegan di Remotion dari keyframes (1080×1920 atau 1920×1080 sesuai lock,
  24/30fps), gerakan kamera sinematik + grain; segmen masuk `videos/` dan diverifikasi ffprobe.

### Tahap 6 — Voice-Over & Sound Design
- VO_SCRIPT.md: naskah narasi/dialog dalam bahasa Fase 0, gaya bercerita sinematik — **tidak berbicara terus-menerus
  (±60% durasi)**, 1–2 kalimat di awal adegan, beri ruang hening untuk akting & musik. Adegan klimaks emosional boleh
  tanpa VO sama sekali ("biarkan aktor bernapas").
- Rekam VO: TTS (suara realistis) atau user merekam sendiri di luar; file per adegan ke `audio/`.
- Sound design per adegan dicatat di STORY.md: ambience lokasi nyata (hujan, jalan, ruangan), foley sederhana,
  musik underscore — semua di-mix di bawah VO/dialog.

### Tahap 7 — Editing MP4 Final (ffmpeg)
- **Verify sebelum mix:** cek stream audio clip asli dengan ffprobe — jangan berasumsi clip bisu. Audio bawaan yang
  berguna dipertahankan dan di-duck di bawah VO.
- **Normalize dulu:** semua clip ke resolusi standar rasio terkunci (16:9 → 1920×1080 atau 1280×720; 9:16 → 1080×1920
  atau 720×1280), **24fps** khas film, baru concat.
- Mix VO per timecode VO_SCRIPT.md, loudness wajar, fade musik di penutup.
- Verify file final dengan ffprobe: rasio benar, fps 24, stream audio ada, total durasi ≈ target Fase 0.

## Output Contract
- `STORY.md` — story bible + screenplay yang sudah dikunci user
- `character_sheets/` — sheet photorealistic setiap aktor, rasio terkunci
- `keyframes/` — K1..K(N+1), rasio terkunci
- `MEGA_PROMPTS.md` — mega prompt per adegan (dengan semua LOCK utuh)
- `videos/` — clip/segmen per adegan, terverifikasi
- `audio/` — VO per adegan + aset sound design
- `VO_SCRIPT.md` — naskah VO + timecode, bahasa Fase 0
- MP4 final: rasio terkunci, 24fps, ada audio, terverifikasi ffprobe, durasi ≈ target

Struktur folder proyek: `~/workspace/live-action-drama/<judul-proyek>/`.

## Operating Rules
1. Fase 0 wajib lengkap (durasi, bahasa, rasio, jalur Tahap 5) sebelum Tahap 1.
2. PHOTOREALISTIC LOCK + ACTOR LOCK ditempel UTUH ke setiap prompt generate — tanpa pengecualian.
3. Character sheet selalu ref pertama. Face drift / terlihat kartun = tolak & generate ulang.
4. Continuity: frame akhir adegan N = frame awal adegan N+1; perubahan penampilan aktor hanya bila tertulis di cerita.
5. **Realism law (dari skill asli, dipertahankan):** setiap aksi fisik kecil harus sesuai mekanisme dunia nyata
   (membuka pintu sebelum lewat, hujan membasahi pakaian rambut secara progresif, cahaya berubah sesuai waktu) —
   tidak ada aksi tidak logis, dan tidak ada fisika kartun.
6. Akting realistis: satu aksi utama per clip, mikro-ekspresi > gerakan besar; hindari gestur teatrikal berlebihan.
7. Verify sebelum mix; normalize sebelum concat; angka yang dilaporkan ke user (jumlah adegan, durasi, resolusi)
   berasal dari output tool/ffprobe nyata, bukan tebakan.
8. Kejujuran jalur: hasil Jalur B selalu disebut stills film; jangan pernah menyebutnya video live-action bergerak.
   Jangan pernah menawarkan mengganti gaya ke animasi sebagai "solusi" tanpa persetujuan eksplisit user.
