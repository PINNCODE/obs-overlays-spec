# OBS Overlays Spec (anti-clone)

[Español → README.es.md](README.es.md)

**Open-source playbook** so **any** streamer can build a full OBS overlay pack with an AI — without ending up with the same centered HUD every time.

This repo is **not** a ready-made Notblu skin. It is the SPEC + master prompt so **your** agent invents layout, hierarchy, and positions from **your** art and assets.

### Works with any AI

ChatGPT, Claude, Gemini, Grok, Cursor, Copilot, local models — **any** assistant that can read a long Markdown file and follow steps. Nothing here is locked to one product.

## What to download

You only need **two files** from this repo (or clone the whole thing):

| File | What it is |
|---|---|
| [`docs/SPEC_OVERLAYS_OBS.md`](docs/SPEC_OVERLAYS_OBS.md) | Full instructions the AI must follow (source of truth) |
| [`prompts/PROMPT_MASTER.md`](prompts/PROMPT_MASTER.md) | Short prompt you paste first (English). Spanish twin: [`PROMPT_MAESTRO.md`](prompts/PROMPT_MAESTRO.md) |

Optional: look at [`examples/notblu/`](examples/notblu/) for **one** historical result — **do not copy** that layout.

## Master prompt (copy-paste)

Fill in the `<…>` with **your** channel data, attach `SPEC_OVERLAYS_OBS.md` + your images (I1–I9), then paste this into any AI:

```
Act as art director and technical producer for OBS overlays. Follow SPEC_OVERLAYS_OBS.md
to the letter (phases 0–7, with an approval CHECKPOINT at the end of each phase).

HARD REQUIREMENT — UNIQUENESS (anti-clone): invent layout, hierarchy, and placement from
concept art (I1), YES/NO references (I9), textures (I5), logo/avatar, and the style bible.
FORBIDDEN to reuse coordinates, outer sizes, or composition from Appendix A (Notblu) or any
previous channel. Same checklist of element *types* = OK; same *geometry* = FAIL. In Phase 3
propose 2–3 layout directions with mocks; the user picks one; then write a unique layout.json
and generate the PNGs.

Channel: <NAME> | Handle: @<handle> | Platform: <YouTube/Twitch/...> | Language: <en-US>
Content (general, no fixed game list): <...> | Tone: <adj1, adj2, adj3> | Style: <...>
Desired/forbidden colors: <...> | Font: <... or "you choose, free license covering needed glyphs">
OBS: <version>, UI in <language>, current canvas <WxH> | Install path: C:\<Channel>_OBS
Attached: concept art (I1), character (I2, optional), logo (I3, optional), palette (I4, optional),
textures (I5), avatar options (I6), channel screenshot (I7), webcam photo (I8), YES/NO refs (I9).

Technical rules: canvas 1920x1080; each element as a separate transparent PNG; camera cutouts
(if used) 384x216 / 480x270 / 1200x675 as aspect contracts — frame position free per layout;
chat = frame + browser source only; no texts that go stale; animations by code as WebM VP9
yuva420p (APNG fallback), no AI video; 12 s looping background; stinger 1.5 s (cut 750 ms)
and 0.8 s (cut 400 ms), with and without audio; importable JSON collection (Loop, muted,
Point filter); numbered ZIPs; README with canvas 1920x1080 as FIRST step and chat/camera
coords taken from THIS project's layout.json.
Start with Phase 0: give me any missing questionnaire answers and your summary (include
visual foci from I1).
```

Same text lives in [`prompts/PROMPT_MASTER.md`](prompts/PROMPT_MASTER.md) if you prefer a dedicated file.

## How to use it (3 steps)

1. Download / open `docs/SPEC_OVERLAYS_OBS.md` and copy the master prompt above (or from `prompts/PROMPT_MASTER.md`).
2. In **any** AI: attach the SPEC file, paste the prompt, fill your channel fields, upload your refs (I1–I9).
3. Start at **Phase 0**. Do not jump to PNGs before choosing a layout direction.

## What it solves

- OBS 1920×1080 overlays ready to import (JSON scene collection)
- Separate PNGs with alpha, WebM VP9 animations with alpha, stinger, branding
- **Uniqueness is required:** in Phase 3 the AI proposes **2–3 layout directions** with mocks; you pick one; then it writes a unique `layout.json`. Same *types* of elements ≠ same geometry.

## Anti-clone validation (summary)

| Check | Status |
|---|---|
| Fixed coords like “title at (540,282)” as a recipe | **Removed** from the operating flow |
| Camera / chat position | Free via this project’s `layout.json` |
| 16:9 camera cutouts | Technical aspect contract, not a layout template |
| Notblu appendix | Historical example only — **NEVER copy** |
| QA | Uniqueness checkboxes + mocks before PNGs |

If two channels built with this SPEC look like siblings in a screenshot, the agent **failed**.

## Repo contents

```
docs/SPEC_OVERLAYS_OBS.md     ← full SPEC (source of truth; currently Spanish)
docs/CHANGELOG_anti_clon.md   ← what changed vs the “cloneable” version
prompts/PROMPT_MASTER.md      ← English prompt (also copied above)
prompts/PROMPT_MAESTRO.md     ← Spanish prompt
examples/notblu/              ← screenshots of ONE example (not a template)
README.es.md                  ← Spanish README
LICENSE                       ← MIT
```

> Note: the full SPEC body is still in Spanish. README + master prompts are bilingual so you can run the flow in EN or ES.

## Who it’s for

YouTube / Twitch / Kick creators who want a serious OBS pack (Starting Soon, Just Chatting, Game, BRB, Ending + stinger) generated with AI — with their **own** identity.

## Credits

Built from **Notblu** ([@notblu14](https://www.youtube.com/@notblu14)) work with Grok Bot. Visuals in `examples/notblu/` are historical reference only.

## License

MIT — use it, fork it, improve it. If you publish a pack made with this SPEC, linking this repo helps more people avoid cloning the same layout.
