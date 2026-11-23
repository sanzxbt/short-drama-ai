# AI Live-Action Short Drama Pipeline

An agent skill for making **photorealistic live-action AI short dramas** — from a raw idea to a finished MP4.

> Skill untuk bikin drama pendek AI **live-action photorealistic**: dari ide mentah sampai MP4 final.

## What it does

A 7-stage production pipeline with strict consistency locks:

1. **Character reference sheets** — photorealistic actor sheets (face, body, expressions)
2. **Story bible & screenplay** — real film script format, actor identity locks
3. **Cinematic keyframes** — film-still keyframes, match-cut continuity between scenes
4. **Mega prompts per scene** — copy-paste-ready first-frame / last-frame video prompts
5. **Video per scene** — generate externally (Veo / Kling / Flow) and assemble here, or stills-film fallback
6. **Voice-over & sound design** — VO script, ambience, music mix
7. **Final edit** — normalize, concat, mix, verify (ffmpeg/ffprobe)

Hard rules baked in: PHOTOREALISTIC LIVE-ACTION LOCK (no cartoon/anime/3D look), ACTOR LOCK (same face, hair and wardrobe in every scene), STRICT ASPECT RATIO LOCK (16:9 or 9:16 for the whole project).

## Install

With the skills CLI:

```
npx skills add sanzxbt/short-drama-ai
```

Or manually: copy `SKILL.md` into your agent's skills folder, e.g. `~/.claude/skills/ai-live-action-short-drama/SKILL.md` (Claude Code) or `~/.agents/skills/ai-live-action-short-drama/SKILL.md`.

## Credits & license

Adapted from **Muse-animated-short-film** by Dang Huu Son ([sonlovinbot-team](https://github.com/sonlovinbot-team/Muse-animated-short-film), MIT) — the full 7-stage workflow and operating rules are retained; the visual direction was reworked from 2D animation to photorealistic live-action cinematic drama.

MIT License.
