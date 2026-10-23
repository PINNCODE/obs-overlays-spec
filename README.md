# OBS Overlays Spec (anti-clone)

[Español → README.es.md](README.es.md)

**Open-source playbook** so **any** streamer can build a full OBS overlay pack with an AI — without ending up with the same centered HUD every time.

This repo is **not** a ready-made Notblu skin. It is the SPEC + master prompt so your agent invents layout, hierarchy, and positions from **your** art and assets.

## What it solves

- OBS 1920×1080 overlays ready to import (JSON scene collection)
- Separate PNGs with alpha, WebM VP9 animations with alpha, stinger transition, branding
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

## How to use it (3 steps)

1. Clone or download this repo.
2. Open your favorite AI and attach [`docs/SPEC_OVERLAYS_OBS.md`](docs/SPEC_OVERLAYS_OBS.md).
3. Paste [`prompts/PROMPT_MASTER.md`](prompts/PROMPT_MASTER.md) (or the Spanish [`PROMPT_MAESTRO.md`](prompts/PROMPT_MAESTRO.md)), fill in your channel data, and upload your images (I1–I9 from the SPEC).

Start at **Phase 0**. Do not jump to PNGs before choosing a layout direction.

## Repo contents

```
docs/SPEC_OVERLAYS_OBS.md     ← full SPEC (source of truth; currently Spanish)
docs/CHANGELOG_anti_clon.md   ← what changed vs the “cloneable” version
prompts/PROMPT_MASTER.md      ← English prompt ready to paste
prompts/PROMPT_MAESTRO.md     ← Spanish prompt ready to paste
examples/notblu/              ← screenshots of ONE example (not a template)
README.es.md                  ← Spanish README
LICENSE                       ← MIT
```

> Note: the full SPEC body is still in Spanish. The README + master prompts are bilingual so you can run the flow in EN or ES. An English SPEC translation can be added later.

## Who it’s for

YouTube / Twitch / Kick creators who want a serious OBS pack (Starting Soon, Just Chatting, Game, BRB, Ending + stinger) generated with AI — with their **own** identity.

## Credits

Built from **Notblu** ([@notblu14](https://www.youtube.com/@notblu14)) work with Grok Bot. Visuals in `examples/notblu/` are historical reference only.

## License

MIT — use it, fork it, improve it. If you publish a pack made with this SPEC, linking this repo helps more people avoid cloning the same layout.
