# Master prompt (paste into your AI)

Also attach [`docs/SPEC_OVERLAYS_OBS.md`](../docs/SPEC_OVERLAYS_OBS.md) and your images (I1–I9).

Fill in the `<…>` with **your** channel data.

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
