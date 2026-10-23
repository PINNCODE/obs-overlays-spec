# CHANGELOG — OBS overlays SPEC (anti-clone)

Changes requested by TRUJILLO: applying the SPEC with another AI produced something almost identical to Notblu. The agent must now **play** with design and placement based on this project’s art/assets.

## What changed

- **§0.1 Uniqueness principle (anti-clone):** forbidden to reuse coords/sizes/composition from Appendix A or other channels; same checklist of *types* OK, same *geometry* = fail; if two projects look like siblings in a screenshot, the agent failed.
- **§4.3:** fixed-pixel table (~762x210, etc.) became **element roles** with guides; outer sizes free based on typography and art.
- **§4.4 / §4.5:** only 16:9 **cutout** contracts (384×216 / 480×270 / 1200×675) and chat as a frame; **position free** via this project’s `layout.json` (no Notblu coords).
- **§5:** per-scene coordinate recipes removed; now **composition rules** + 2–3 layout directions with mocks before locking.
- **Phase 3:** explicit flow explore assets → 2–3 mocks → pick a direction → unique `layout.json` → generate PNGs.
- **§9 / LEEME:** chat and camera must cite **this** project’s `layout.json`, not Notblu hardcodes.
- **§10 QA:** uniqueness checkboxes (layout derived from art; different from Notblu; mocks first).
- **§13 prompts:** uniqueness as a hard requirement; invent layout; don’t use Appendix A; propose 2–3 layouts.
- **Appendix A:** big warning — Notblu historical example **only**; don’t copy coords/sizes/composition; some positions omitted on purpose so they’re harder to clone.

## What stayed (technical)

- Canvas 1920×1080, separate RGBA PNGs, no texts that go stale, code-driven animation + WebM VP9 yuva420p (+ APNG), stingers 1.5 s/750 ms and 0.8 s/400 ms, JSON collection, Point filter, loop, checkpoints, questionnaire, I1–I10, style bible, QA/LEEME/ZIPs/ffmpeg.

Spanish twin: [`CHANGELOG_anti_clon.md`](CHANGELOG_anti_clon.md).
