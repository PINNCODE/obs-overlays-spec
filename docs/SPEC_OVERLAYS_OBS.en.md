# SPEC: End-to-end OBS overlay package (instructions for an AI agent)

> **English edition.** The Spanish original is [`SPEC_OVERLAYS_OBS.md`](SPEC_OVERLAYS_OBS.md). This file is the English twin. Same rules apply, including the anti-clone principle (§0.1).
>
> Operating document. Read it fully before you start. It is written so **another AI agent** can run it with **any streamer** and **any visual style**.
> The Notblu values (Appendix A) are **one resolved example for a specific channel**. **Never copy its style, colors, texts, coordinates, outer sizes, or composition**, unless the user explicitly asks for that same channel.

---

## 0. Role and objective

**Role:** you are art director and technical producer of streaming overlays. You design the channel’s visual identity and produce, **with code**, all OBS assets: static, animated, the scene collection, and the transition.

**Objective:** deliver a pack that installs in OBS in under 10 minutes and does not break when channel content changes. It includes:

1. Visual identity: approved style bible.
2. Channel branding: banner, avatar, and description.
3. OBS elements as **separate transparent PNGs**.
4. Animated versions in **WebM VP9 with alpha** (APNG as alternative) and a looping background.
5. A **JSON scene collection**, ready to import.
6. A **stinger transition** with audio and another version without audio.
7. LEEME / README files (instructions for the end user, in their language) and a QA report.

**Non-negotiable principles:**
- No game names, seasons, or data that go stale inside the graphics. Channel content changes.
- Each element goes in **its own file**. Never deliver only “flattened” scenes.
- Animations are generated **by code**. Do not use AI video for anything that needs transparency (see §6.1).
- The OBS canvas is **1920x1080**. All coordinates refer to it.
- Ask for approval at every **CHECKPOINT** (🛑) before continuing.
- **Uniqueness:** this channel’s layout must be invented from its assets; do not reuse geometry from other projects (§0.1).

---

## 0.1 Uniqueness principle (anti-clone)

This SPEC describes **what types of elements** to produce and **which technical contracts** to meet. It does **not** describe a layout to clone.

**Forbidden:**
- Reusing coordinates, outer sizes, or composition from Appendix A (Notblu) or any previous channel as a template.
- Defaulting to centering title + text + bar + user like Notblu, or always putting the camera bottom-left in Game, just because “that’s how the example turned out.”
- Treating Appendix A measures such as `~762x210`, `540x723 @ (1332,168)`, etc. as mandatory sizes.

**Mandatory:**
- Invent hierarchy, placement, and outer sizes from: concept art (I1), YES/NO references (I9), textures (I5), logo/avatar, and the approved style bible.
- The same checklist of element *types* is fine; the same *geometry* is a failure.
- If two projects made with this SPEC would look like siblings in a screenshot (same composition, same anchors, same title/frame/chat proportions), **the agent failed**.

In Phase 3 you must **play with the design**: explore, propose 2–3 distinct directions, and let the user choose. Each channel’s `layout.json` is unique.

---

## 1. Required inputs

### 1.1 Questionnaire (do this at the start; do not proceed without answers marked required)

| # | Question | Required | Notes |
|---|---|---|---|
| 1 | Channel name, exactly as it should appear (caps, accents) | Yes | Check that the chosen font has all characters (Á, É, Í, Ó, Ú, Ñ, ¡, ¿, and any other glyphs the channel needs). |
| 2 | Handle (@user) | Yes | Goes on user tags. |
| 3 | Platform(s): YouTube, Twitch, Kick… | Yes | Changes the chat popout and alerts. |
| 4 | Content type (in general terms: “varied games”, “talks”, “art”) | Yes | Used for the channel description. **Do not** ask for a fixed list of games. |
| 5 | Tone and personality (3 to 5 adjectives) | Yes | Example: “nocturnal, retro, relaxed”. |
| 6 | Desired visual style (pixel art, flat, anime, minimalist, vaporwave…) | Yes | Confirm it with the concept art. |
| 7 | Colors: favorites and forbidden | Yes | If none, propose a palette from the concept art. |
| 8 | Preferred font(s), or “you choose” | No | Must have a free license (OFL or Google Fonts) and cover the overlay language (e.g. Spanish accents if needed). |
| 9 | Language of overlay texts and of the LEEME / README | Yes | |
| 10 | What changes often? (game, schedule, goals, sponsors) | Yes | Everything that changes goes as **editable OBS text** or a browser source, never inside a PNG. |
| 11 | Required scenes (minimum: Starting Soon, Just Chatting, Game, BRB, Ending) | Yes | Ask about extra scenes. |
| 12 | Camera: do they use one? in which scenes? preferred size? | Yes | Defines which frame **roles** to make (§4.3); position is decided in Phase 3. |
| 13 | Alert service (Streamlabs, StreamElements, native) | No | Defines the alert format. |
| 14 | OS, OBS version, and **OBS UI language** | Yes | Instructions must use the menu names of **their** language (see §9). |
| 15 | Current OBS canvas resolution (Settings > Video) | Yes | If it is not 1920x1080, change it **before** importing. |
| 16 | Do they want audio on the transition? | No | Always deliver both versions: with audio and without audio. |
| 17 | Do they have a mascot or character? What avatar do they use? | No | |
| 18 | Path where they will install the files (default `C:\<Channel>_OBS`) | Yes | The path is written into the collection JSON. |

### 1.2 Mandatory image list (concept art and references)

Ask for these **before** designing. Without the required ones there is no solid concept. These images **drive** the composition: they are not optional decoration.

| # | Image | What it is for | Minimum specs | Required |
|---|---|---|---|---|
| I1 | **Main concept art or moodboard** (1 to 5 images) | Fixes style, atmosphere, lighting, palette, **and attention foci** (where the eye goes → zone to leave free or to frame). May serve as a base background (if the user has the rights). | ≥ 1920x1080 (ideal ≥ 2560x1440), good-quality PNG or JPG, no watermarks | **Yes** |
| I2 | **Character or mascot** (if any) | For the avatar, avatar ring, and Ending scene. | ≥ 1024x1024, plain or transparent background, front or 3/4 view | Optional (required if they want a character) |
| I3 | **Logo or title treatment** (if any) | For the channel title. If none exists, you design it. | Vector (SVG, PDF) or PNG ≥ 2000 px wide with transparency | Optional |
| I4 | **Palette swatch** or HEX list | Fixes exact colors. If none, extract from I1 and ask for approval. | Image with color blocks or `#RRGGBB` list | Optional (generated in Phase 1) |
| I5 | **Texture or background references** | For the background pattern, type of clouds, stars, noise, *dithering*, scanlines. Influences HUD density and margins. | 2 or 3 images, any size | Optional, recommended |
| I6 | **Avatar options** (2 or 3) | So the user picks one. The chosen one is used across the pack. | ≥ 800x800 each, on a single sheet labeled "OPCIÓN 1/2/3" or "OPTION 1/2/3" | **Yes** (if they don’t have them, generate and ask them to choose) |
| I7 | **Current channel screenshot** | To see current banner and avatar, circular crop of the avatar, and what to keep. | Full desktop channel page screenshot (and mobile if possible) | **Yes** |
| I8 | **Webcam or setup photo** | To know camera aspect ratio (16:9 or 4:3), framing, and which frame types fit. | Webcam capture as it appears in OBS | **Yes** if they use a camera |
| I9 | **"YES" and "NO" references** | Overlays they like and overlays they don’t want (composition, density, chat left/right, etc.). Avoids useless iterations and cloning layouts they hate. | 2 or 3 of each, with a short note on why | **Yes** (at least 1 of each) |
| I10 | Screenshot of their current OBS (Settings > Video and the scene list) | To detect canvas resolution and UI language. | Screenshot | Recommended |

> If the user uploads AI-generated or third-party images, confirm they can use them commercially on their channel.

---

## 2. Work phases

Each phase has: steps, acceptance criteria (AC), and a 🛑 CHECKPOINT. **Do not advance phases without the user’s explicit approval.**

### Phase 0: Collection
1. Send the questionnaire (§1.1) and the image list (§1.2).
2. Review each image: resolution, rights, and style coherence across all of them.
3. Write a 5-line summary of what you understood (style, tone, tentative palette, scenes, camera) **and a note on visual foci from I1** (where the art pushes the eye).

**AC:** required answers complete; images I1, I6, I7, I8, and I9 received; OBS canvas known.
🛑 **CHECKPOINT 0:** "Is this summary correct?"

### Phase 1: Visual identity (style bible)
1. Extract the palette from I1 or I4: 6 to 10 colors with a role for each (background, accent 1, accent 2, text, shadow, highlight).
2. Choose the font (free license, compatible with the overlay language) and define its base size.
3. Define the “construction rule”: pixel grid and *nearest neighbor* scaling for pixel art, or radii, thicknesses, and shadows for a vector style.
4. Design the title treatment (channel logo) in 2 or 3 variants.
5. If there are avatar options, the user picks one.
6. Fill the style bible template (§3) and generate a sample sheet (palette, font, title, and an example frame).

**AC:** palette has HEX values; font displays all needed accents/glyphs; title is readable at 25% size; an avatar is chosen.
🛑 **CHECKPOINT 1:** approval of the style bible, title, and avatar.

### Phase 2: Channel branding
1. Banner 2560x1440 with everything important inside the centered **safe zone of 1546x423** (x 507–2053, y 508–931).
2. Mobile crop preview (1546x423) and desktop preview.
3. Avatar 800x800 with circular crop preview.
4. Channel description: about content and personality. **No lore, no fixed game names**, and schedule only if the user asks.

**AC:** nothing is cropped in the mobile view; avatar reads in a 98 px circle; description is under 1000 characters.
🛑 **CHECKPOINT 2:** approval of banner, avatar, and description.

### Phase 3: Static OBS elements (composition first)
1. **Explore composition from the assets** (I1, I5, I9, logo, avatar, style bible): identify foci, dense/empty zones, and biases from YES/NO references.
2. Propose **2–3 distinct layout directions** (§5), with composite scene mocks (Starting Soon + Game + Just Chatting at minimum). Example directions: asymmetric camera on the left, vertical stack, HUD only in corners, centered title with orbital accents, chat left vs right, corner variants for the game HUD. They must be anchored to the moodboard, not to Notblu.
3. 🛑 At this phase’s checkpoint the user **picks one direction**. Only then do you lock the layout.
4. Write a **channel-unique** `layout.json` (x, y, w, h of each element on the 1920x1080 canvas) and the layer order.
5. Generate each §4.3 element as a **separate PNG RGBA**, cropped to its content with alpha 0 outside the art. *Outer* sizes grow or shrink with typography and art; **do not** copy Appendix A measures.
6. Camera frames (if used) have a **100% transparent cutout** with the aspect contract of §4.4. The chat panel is **frame only**; the real chat sits on top as a browser source.
7. Generate full scenes only as visual reference, aligned with `layout.json`.
8. Write the `LEEME.txt` with the element list, sizes, **positions taken from THIS project’s `layout.json`** (never hardcoded Notblu coordinates), and layer order.

**AC:** passes §10 QA for statics; no texts that go stale; camera cutouts (if any) meet the 16:9 contract; layout is derived from this project’s art and **differs in composition** from the Notblu example.
🛑 **CHECKPOINT 3:** approval of the chosen layout direction and of the reference scenes (Starting Soon, Just Chatting, Game, BRB, Ending).

### Phase 4: Animation
1. Animate each element **by code** from its static version (§6).
2. Export WebM VP9 with `yuva420p` and APNG as alternative.
3. Make a perfectly looping animated background (12 s recommended), without alpha (MP4 H.264 + WebM).
4. Verify alpha by **decoding** the WebM (do not trust the encoder alone).
5. Make preview GIFs and a `LEEME_ANIMADOS.txt` (or English README for animated assets).

**AC:** loops with no jump (first frame matches the frame after the last); cutouts have alpha 0 in every frame; same measures as the static (or new coordinates documented in `layout.json`); no deformations.
🛑 **CHECKPOINT 4:** approval of the preview GIFs.

### Phase 5: OBS scene collection
1. Generate `<Channel>_coleccion_OBS.json` (OBS 28+ collection format, `resolution` 1920x1080).
2. Absolute paths to `C:/<Channel>_OBS/…` (or the path the user gave).
3. On every media source: `looping: true`, `restart_on_activate: true`, and `muted: true`. Scale filter "Punto" / "Point" on all pixel art.
4. Share "Cámara" / "Camera" across scenes (one source, reused) and fit it to the cutout with *bounds* per `layout.json`.
5. Render each scene from the JSON (with test values for camera and game) to verify positions.
6. Write `LEEME_COLECCION.txt`: chat/camera coordinates = **those from THIS project’s `layout.json`**.

**AC:** JSON imports without errors; the 5 scenes fill the canvas; no missing files.
🛑 **CHECKPOINT 5:** the user imports the collection and confirms with a screenshot.

### Phase 6: Stinger transition
1. Generate the transition per §7: main version, fast version, with audio and without audio.
2. Verify frame by frame that the first and last frames have alpha 0 and that the cut frame is 100% opaque.
3. Write `LEEME_TRANSICION.txt` with the transition point in ms.

**AC:** coverage report (§7) is met and audio does not clip (peak ≤ −1 dBFS).
🛑 **CHECKPOINT 6:** the user tests it in OBS.

### Phase 7: Delivery
Package per the manifest (§12) and deliver the QA report.

---

## 3. Style bible template (fill in Phase 1)

```yaml
canal: "<NAME>"
handle: "@<handle>"
estilo: "<pixel art / flat / anime / ...>"
tono: ["<adj1>", "<adj2>", "<adj3>"]
paleta:                    # 6-10 colors, each with a role
  fondo_oscuro:  "#RRGGBB"
  fondo_medio:   "#RRGGBB"
  acento_1:      "#RRGGBB"   # borders / main neon
  acento_2:      "#RRGGBB"   # alerts / LIVE
  texto:         "#RRGGBB"
  brillo:        "#RRGGBB"
  sombra:        "#RRGGBB"
degradados: ["<list of bands for the title, if applicable>"]
fuente_titulos: "<name> (<license>)"
fuente_texto:   "<name> (<license>)"
construccion:
  rejilla_px: <n>          # pixel art: 1 cell = n px; vector: null
  escalado: "nearest"      # pixel art; "bilinear" for vector
  grosor_borde_px: <n>
  radios_px: <n or 0>
  sombra: "<offset, color>"
  efectos: ["scanlines", "dithering", "glow", ...]
titulo: "<treatment description + reference file>"
avatar_elegido: "<option N>"
textos_fijos: ["STARTING SOON", "BE RIGHT BACK", "THANKS FOR WATCHING", ...]
textos_variables_en_OBS: ["current game", "goal", ...]   # never inside a PNG
prohibido: ["<colors/styles/elements they do NOT want>"]
direccion_layout: "<name of the direction chosen in Phase 3>"  # unique per channel
```

---

## 4. Exact technical specs

### 4.1 Canvas and branding

| Asset | Size | Format | Notes |
|---|---|---|---|
| OBS canvas (Base and Output) | **1920x1080** | — | Set it **before** importing. All coordinates are for this canvas. |
| YouTube banner | 2560x1440 | PNG + JPG (≤ 6 MB) | Centered **1546x423** safe zone (x 507–2053, y 508–931). |
| Banner mobile preview | 1546x423 | PNG | Safe-zone crop, for review. |
| Avatar | 800x800 | PNG | Check it works in a circle (preview with circular mask). |
| Description | ≤ 1000 characters | text | About content; no lore or fixed games. |

### 4.2 Formats, alpha, fps, and loops

| Type | Primary format | Alternative | Alpha | FPS | Duration |
|---|---|---|---|---|---|
| Static element | PNG RGBA | — | Yes (alpha 0 outside the art) | — | — |
| Static background | PNG RGB 1920x1080 | — | No | — | — |
| Animated element | **WebM VP9 `yuva420p`** | APNG (infinite loop) | Yes | 24 | 3 s loop (72 frames); 4 s for spinning elements |
| Animated background | MP4 H.264 `yuv420p` | WebM VP9 `yuv420p` | No | 24 | **Perfect 12 s loop** (288 frames) |
| Alert | WebM VP9 with alpha | PNG | Yes | 24 | Plays once (intro and hold), no loop |
| Stinger transition | **WebM VP9 `yuva420p`** with audio | `_mudo` version | Yes | 60 | 1.5 s main and 0.8 s fast |
| Audio | WAV 48 kHz | OGG | — | — | Original or licensed |
| Preview | GIF / MP4 | — | — | — | Review only |

Reference ffmpeg commands:
```bash
# Animated element with alpha (lossless so pixel art stays clean)
ffmpeg -framerate 24 -i f_%04d.png -c:v libvpx-vp9 -pix_fmt yuva420p -auto-alt-ref 0 -lossless 1 -row-mt 1 out.webm
# Background (no alpha)
ffmpeg -framerate 24 -i bg_%04d.png -c:v libx264 -preset slow -crf 14 -pix_fmt yuv420p -tune animation -movflags +faststart fondo.mp4
ffmpeg -framerate 24 -i bg_%04d.png -c:v libvpx-vp9 -crf 18 -b:v 0 -row-mt 1 -pix_fmt yuv420p fondo.webm
# Alpha verification: decode FORCING libvpx-vp9 (the native decoder drops alpha)
ffmpeg -c:v libvpx-vp9 -i out.webm d_%04d.png
```

### 4.3 Element roles (recommended minimum) — outer sizes free

Produce these **roles** when they apply. The *types* checklist is shared; the **geometry is not**. Outer sizes grow or shrink to fit typography, art, and the chosen layout direction. **Do not copy** Appendix A-style measures like `~762x210`.

| Role / typical file | Guide (not a fixed size) | Notes |
|---|---|---|
| `fondo.png` / `fondo_animado.mp4` | Exact **1920x1080** | Background for Starting Soon, BRB, Ending, Just Chatting (and variants if needed). |
| `fondo_<variant>.png` | 1920x1080 | Optional. |
| `marco_hud_pantalla.png` | 1920x1080, transparent center | Corners/rules/borders per the art; do not clone Notblu’s HUD. |
| `titulo_<channel>.png` | Fits logo/type; readable at ~25% | Main hierarchy; size and proportion come from branding. |
| `etiqueta_<channel>_pequena.png` | Compact for game HUD | Reduced logo version; do not inherit proportions from another channel. |
| `etiqueta_transmision.png` | Corner strip or chip | Text like "<CHANNEL> // LIVE" if the language calls for it. |
| `estado_conectando.png` / `estado_en_pausa.png` / `estado_fin.png` | Status chips/bars | Width follows the text; position follows the layout direction. |
| `texto_empezando_pronto.png`, `texto_volvemos.png`, `texto_gracias_por_ver.png` | Scene texts | Invented hierarchy (do not stack center by default). |
| `texto_suscribete.png` (or "FOLLOW ME" / "SUBSCRIBE") | CTA | Visual weight consistent with I9. |
| `barra_carga.png` | Bar / progress | Free form (segmented, line, etc.) per style. |
| `usuario_<handle>.png` | Handle chip | |
| `avatar_anillo.png` (or other avatar frame) | Per the art | Only if Ending / branding call for it. |
| `marco_camara_pequeno.png` / `marco_camara.png` / `marco_camara_grande.png` | Outer free; **cutout** exact 16:9 (§4.4) | Choose **which** frame types to use and **where** they go; using all three is not mandatory. |
| `etiqueta_en_vivo.png`, `etiqueta_cam.png` | Tabs / badges | |
| `barra_alertas.png` | Guide with transparent zone | Width and anchor per layout (top, side, etc.). |
| `panel_chat.png` | **Frame only**; useful interior for the popout | Size and side (left/right) per the chosen direction. |
| `encabezado_charlando.png` | Just Chatting header | Optional per hierarchy. |
| `alerta_seguidor.png` | Documented empty space (x,y,w,h) for the name | |

### 4.4 Camera cutout contracts (16:9, transparent) — free position

If you use a frame type, the **cutout** must be a 100% transparent 16:9 rectangle with these contract sizes (you define the PNG’s *outer* size and the cutout *offset* inside the file, consistent with the art border):

| Frame type (if used) | **Cutout** size (contract) |
|---|---|
| Small | **384×216** |
| Normal | **480×270** |
| Large | **1200×675** |

- **Where** the frame sits on the canvas is decided by Phase 3 composition → `layout.json`.
- Camera position on the canvas = frame position + cutout offset inside the PNG. The camera goes **under** the frame in the sources list.
- If the webcam is 4:3, use crop or *bounds* “scale to outer bounds” with crop; never stretch it.
- Producing all three types is not mandatory: produce what the user and layout need.

### 4.5 Chat
- `panel_chat.png` is **frame only**. Outer size, side (left/right), and coordinates come from **this** project’s `layout.json`.
- Document the useful interior (x, y, w, h of the transparent area) for the browser source.
- Browser source with the platform’s chat popout and CSS `body { background: rgba(0,0,0,0); }`.
- In LEEME and import: **use `layout.json` coords**, not Notblu values.

---

## 5. Composition rules per scene (1920x1080 canvas)

**There are no fixed-coordinate recipes.** Positions live in the channel’s unique `layout.json`. Document layer order **bottom → top** (in OBS, the last in the list sits on top in the sources panel).

### 5.1 Before locking the layout
1. Propose **2–3 distinct layout directions**, anchored to the moodboard (I1/I5/I9), for example:
   - Asymmetric camera left / right
   - Vertical stack (branding top, content bottom)
   - HUD only in corners (clean center)
   - Centered title with orbital accents
   - Chat left vs chat right
   - Corner variants for the game HUD (per platform norms and the art)
2. Show **composite mocks** of at least Starting Soon, Game, and Just Chatting.
3. The user picks **one** direction at the checkpoint. Only then do you fix `layout.json`.

### 5.2 Derivation from the art
- Locate in I1 where the eye goes; leave that zone freer or deliberately frame it.
- Respect I9 biases (density, chat side, “lots of HUD” vs “minimal”).
- Safe margins: ≥ **12 px** from the edge on all elements; nothing critical within **36 px** of the edge on full-screen scenes (Starting Soon / BRB / Ending / Just Chatting).
- Stream-size readability: titles and CTAs must read in a capture at ~25–50% of the canvas.

### 5.3 Per scene (intent, not a template)
- **Game:** keep the **playable center relatively free**. HUD only where the art and platform norms allow; pick the corner(s) from assets — **not** always camera bottom-left.
- **Starting Soon / BRB / Ending / Just Chatting:** invent hierarchy (title vs avatar vs CTAs vs states) from the branding. **Do not** stack to center by default “like Notblu.”
- **Just Chatting:** decide chat side, camera size, and headers per the chosen direction; keep exact 16:9 cutouts if there is a frame.
- Layer order: documented bottom→top in LEEME and reflected in the JSON.

### 5.4 Failure criterion
If the final mock could be confused with Appendix A in a screenshot (same anchors, same central stack, same chat side and same camera corner), **redesign** before the checkpoint.

---

## 6. Animation and alpha

### 6.1 Why NOT to use AI video (Grok Imagine, Runway, Sora, etc.)
- **No alpha channel.** Overlays need real transparency; a *chroma key* leaves halos and dirty edges, especially on pixel art and neon.
- **Loops don’t close:** the last frame doesn’t match the first and the jump shows.
- **There is morphing:** letters, frames, and the logo melt and change shape between frames. That destroys the brand and the pixel grid.
- **It doesn’t respect measures:** camera cutouts stop being exact.
→ AI video is only acceptable as **motion inspiration** or, at most, for a reviewed opaque background. Never for elements with transparency.

### 6.2 How to animate (by code)
- Start **from the approved static PNG**: the animation modifies layers (glow, offset, masks); it does not redraw the shape.
- Valid effects: stepped neon flicker, scanline sweeping the element, stripe glitch with 1–2 cell chromatic offset, light traveling the border, pulsing corners, blinking cursor, filling "..." dots, filling bar, spinning ring, and background parallax.
- In pixel art, animate “in steps”: 24 fps container, but change every 2 frames (12 fps feel). Motions are multiples of the cell and never subpixel.
- **Perfect loop:** all periodicities must divide the frame count (e.g. 72 frames: periods of 72, 36, 24, 18, 12…). On the 12 s background (288 frames), each parallax layer travels an integer number of tile widths.
- Cutouts (camera, chat, and alerts) are forced to **alpha 0 on every frame**.
- Same measures as the static. If an element grows (cursor, dots), keep the right edge or center fixed and **document the new X in `layout.json`**.
- The animated background is opaque: MP4 and WebM without alpha.
- The alert is the only non-looping animation: intro then hold.

### 6.3 Mandatory verification
Decode the WebM with `-c:v libvpx-vp9` and on every frame check: cutout alpha is 0, alpha outside the art is 0, size is correct, and the difference between frame 0 and the frame after the last equals that between consecutive frames. Save an `anim_report.json`.

---

## 7. Stinger transition spec

| Parameter | Main | Fast |
|---|---|---|
| File | `transicion_<channel>.webm` + `_mudo.webm` | `transicion_rapida.webm` + `_mudo.webm` |
| Resolution / fps | 1920x1080 / 60 | 1920x1080 / 60 |
| Duration | 1.5 s (90 frames) | 0.8 s (48 frames) |
| Codec | VP9 `yuva420p` + Opus audio | same |
| First / last frame | **100% transparent** | same |
| Full coverage | continuous stretch around the cut (e.g. 567–900 ms) | e.g. 317–467 ms |
| **Transition point** | **750 ms** (frame 45) | **400 ms** (frame 24) |
| Content | Visual motif consistent with the style bible (wipe, sparks, logo at the cut, etc.) | Shorter variant, no logo if it doesn’t fit |
| Audio | Whoosh + hit at the cut, peak ≤ −1 dBFS, original | short |

Rules: the cut always falls inside the 100% opaque stretch (alpha 255 on every pixel), never on the edge of the stretch. No *track matte* needed. Deliver a PNG of the cut frame, an MP4 or GIF preview, and a `reporte.json` with per-frame coverage.

OBS setup: Scene Transitions → "+" → Stinger → `.webm` file → Transition point type: Time (ms) → 750 (or 400) → Audio monitoring as preferred → Audio fade style: "Fade to transition point" / "Desvanecer hasta el punto de transición". The "Duration" field does not affect the stinger.

---

## 8. Naming and folder conventions

- Names in lowercase, `snake_case`, no spaces, no accents, and no ñ (`pequeno`, not `pequeño`).
- Same base name for static, animated, and preview: `marco_camara.png` / `marco_camara.webm` / `marco_camara.apng` / `previews/marco_camara.gif`.
- Prefixes: `fondo_`, `marco_`, `etiqueta_`, `estado_`, `texto_`, `barra_`, `panel_`, `alerta_`, `transicion_`, `sonido_`.
- **ZIPs must say what they contain**, with an order number:

```
<channel>_1_branding.zip                  banner, avatar, descripcion.txt
<channel>_2_elementos_png.zip             elementos/ + escenas_completas/ (reference) + layout.json + LEEME.txt
<channel>_3_animados.zip                  *.webm, *.apng, fondo_animado.mp4/.webm, previews/, LEEME_ANIMADOS.txt
<channel>_4_COLECCION_OBS_IMPORTAR.zip    <Channel>_OBS/ (json + webm/mp4 it uses) + LEEME_COLECCION.txt   ← the one to import
<channel>_5_transicion.zip                <Channel>_OBS/transiciones/ (webm, _mudo, sonido/, vista_previa/) + LEEME_TRANSICION.txt
```

Install structure on the user’s PC (no extra intermediate folders):
```
C:\<Channel>_OBS\
  <Channel>_coleccion_OBS.json
  LEEME_COLECCION.txt
  layout.json
  fondo_animado.mp4
  *.webm
  transiciones\
    transicion_<channel>.webm, transicion_<channel>_mudo.webm, transicion_rapida*.webm
    sonido\  vista_previa\
```

---

## 9. OBS setup and import (steps for the end user)

> Write these steps in the language of **their** OBS UI. Put the other language’s name in parentheses, e.g. "Colección de escenas (Scene Collection) > Importar (Import)".
> **Chat and camera coords:** always use those from THIS project’s `layout.json` / LEEME. Do not hardcode Appendix A (Notblu) positions.

1. **FIRST, the canvas:** Ajustes (Settings) > Video: Resolución base (lienzo) / Base (Canvas) Resolution = **1920x1080** and Resolución de salida (escalada) / Output (Scaled) Resolution = **1920x1080**. Apply. *(If you import with another canvas, scenes won’t fill the screen.)*
2. Unzip `<channel>_4_COLECCION_OBS_IMPORTAR.zip` and leave the folder at `C:\<Channel>_OBS` (the JSON must sit directly inside).
3. Colección de escenas (Scene Collection) > Importar (Import) > pick the `.json` > check it > Import.
4. Scene Collection > select "<Channel>". Previous scenes stay in their original collection.
5. If OBS warns about missing files, use "Buscar en directorio" (Search Directory) and point to the folder.
6. Camera: double-click "Cámara" / "Camera" > pick the webcam (one shared source between Just Chatting and Game). Fit it to the cutout per `layout.json`.
7. Game capture: full-screen mode by default. If the game is windowed, switch to capture a specific window.
8. Chat: add a Browser source with the chat popout URL, at the coordinates and useful interior **documented in this pack’s `layout.json` / LEEME**, with transparent-background CSS.
9. Alerts: Browser source with the widget URL, on the top layer of the Game scene (position per layout). Upload `alerta_seguidor.webm` as the alert video.
10. Transition: §7.
11. Test each scene in Studio Mode before going live.

To add loose elements by hand: "+" > Fuente multimedia (Media Source) > local file > check Bucle (Loop). Then right-click > Transform > Edit Transform (alignment: top-left, scale 100%). Finally, right-click > Scale Filtering > **Punto (Point)**.

JSON values the agent must generate: `resolution: {x:1920, y:1080}`; `ffmpeg_source` sources with `is_local_file: true`, `local_file: "C:/<Channel>_OBS/<file>"`, `looping: true`, `restart_on_activate: true`, `close_when_inactive: false`, `muted: true`; on scene items, `scale_filter: "point"` for pixel art; camera with `bounds_type` fitted to **this** layout’s cutout; and desktop and mic audio devices set to "default".

---

## 10. QA checklist (fill and deliver)

**Uniqueness / anti-clone**
- [ ] Layout was derived from **this** project’s art (I1, I5, I9, logo, avatar, style bible), not Appendix A.
- [ ] 2–3 layout directions with mocks were proposed; the user picked one before final PNGs were generated.
- [ ] Composition **differs** from the Notblu example (not the same central stack, not the same default camera/chat anchors).
- [ ] `layout.json` is unique to this channel and LEEME/import use those coords (no Notblu hardcodes).

**Statics**
- [ ] All are PNG RGBA (background may be RGB) and cropped to content.
- [ ] Alpha 0 outside the art; no white or black halos on edges (check over gray, green, and magenta backgrounds).
- [ ] If camera frames exist: cutouts measure exactly 384x216, 480x270, and/or 1200x675 (per types used), with alpha 0.
- [ ] In pixel art, every edge lands on the grid and there are no intermediate pixels from scaling.
- [ ] Accents and Ñ (or other needed glyphs) look correct; no spelling errors.
- [ ] No game names or data that go stale.
- [ ] Reference scenes match `layout.json`.

**Animated**
- [ ] The WebM, when decoded, has alpha (verified with libvpx-vp9).
- [ ] Perfect loop; no jump visible across 3 consecutive cycles.
- [ ] Cutouts have alpha 0 in every frame.
- [ ] Same size as the static, or the new X is documented in `layout.json`.
- [ ] APNG fallback and preview GIF exist.
- [ ] Background is 12 s, opaque, and perfectly looping, in both MP4 and WebM.

**Collection**
- [ ] JSON is valid, has `resolution` 1920x1080, and paths are correct.
- [ ] Loop on, sources muted, Point filter.
- [ ] Camera is a single shared source, fitted to this layout’s cutout.
- [ ] There is a render of each scene from the JSON.
- [ ] The collection ZIP is named so it’s obvious which one to import.

**Transition**
- [ ] First and last frames have alpha 0.
- [ ] Opaque stretch is continuous and contains the cut point (750 / 400 ms).
- [ ] With-audio and without-audio versions exist; audio does not clip.

**Documentation**
- [ ] LEEME files are in the user’s language, with their OBS menu names.
- [ ] First setup step is the 1920x1080 canvas.
- [ ] LEEME_COLECCION cites this project’s `layout.json` for chat/camera (no Notblu coords).

---

## 11. Common errors and fixes

| Problem | Cause | Fix |
|---|---|---|
| Pack looks almost identical to Notblu / another channel | Geometry copied from Appendix A or a previous layout | Back to Phase 3: 2–3 new directions from I1/I9; uniqueness is required (§0.1, §5). |
| Imported scenes don’t fill the screen or look shifted | OBS canvas wasn’t 1920x1080 at import | Settings > Video: Base and Output at 1920x1080 **first**; then delete the collection and re-import. |
| User doesn’t know which ZIP has the JSON | Ambiguous ZIP names | Number the ZIPs and put "COLECCION_OBS_IMPORTAR" in the name (§8). |
| Instructions don’t match the menus | User’s OBS is in another language (e.g. English) | Ask the language (Q14) and write menu names in both languages. |
| Missing files on import | Folder isn’t at the JSON path or there’s an extra intermediate folder | Exact path `C:\<Channel>_OBS\`, or "Search Directory". |
| Blurry pixel art | Bilinear scale filter or non-integer scale | "Point" filter and scales at 100%, 200%, or 300%. |
| WebM shows with a black background | Exported without `yuva420p`, old OBS, or the preview MP4 was chosen | Re-export with `-pix_fmt yuva420p -auto-alt-ref 0`, use OBS 28+, pick the `.webm`. |
| "WebM has no alpha" when verifying | ffmpeg used the native VP9 decoder | Decode with `-c:v libvpx-vp9` before `-i`. |
| AI animation with halos or that “melts” | AI video without alpha and with *morphing* | Discard it and animate by code (§6). |
| Visible jump in the loop | Periods that don’t divide the frame count | Adjust periods (§6.2) and verify frame N against frame 0. |
| Overlay shows a game no longer played | Game name was written into the PNG | Use generic texts; variables go in an OBS text source. |
| Chat covers the frame or has a white background | Browser source misplaced or missing CSS | Place it in the panel interior (coords from **this** `layout.json`) with `background: rgba(0,0,0,0)`. |
| Camera looks stretched | 4:3 webcam forced into a 16:9 cutout | Use *bounds* with crop, not stretch. |
| Scene change is visible before the screen is covered | Transition point falls outside the opaque stretch | Measure coverage and put the cut in the middle of the stretch. |
| Transition audio does or doesn’t play on stream | Audio monitoring misconfigured | "Monitor and Output", "Monitor Only", or use `_mudo`. |
| Banner cropped on mobile | Content outside the 1546x423 zone | Always check the mobile preview. |
| LEEME coords don’t match the JSON | Numbers pasted from Appendix A | Regenerate LEEME from this project’s `layout.json`. |

---

## 12. Final deliverables manifesto

| # | Deliverable | Files | Phase |
|---|---|---|---|
| 1 | Style bible | `style_bible.yaml` + `lamina_estilo.png` | 1 |
| 2 | Branding | `banner_2560x1440.png/.jpg`, `banner_movil_preview.png`, `avatar_800.png`, `avatar_circulo_preview.png`, `descripcion.txt` | 2 |
| 3 | PNG elements | `elementos/*.png`, `escenas_completas/*.png`, unique `layout.json`, Phase 3 direction mocks, `LEEME.txt` | 3 |
| 4 | Animated | `*.webm` (VP9 alpha), `*.apng`, `fondo_animado.mp4/.webm` (12 s), `previews/*.gif`, `LEEME_ANIMADOS.txt`, `anim_report.json` | 4 |
| 5 | OBS collection | `<Channel>_OBS/<Channel>_coleccion_OBS.json` + the assets it uses + `LEEME_COLECCION.txt` + scene renders | 5 |
| 6 | Transition | `transicion_<channel>.webm`, `_mudo.webm`, `transicion_rapida.webm`, `_mudo.webm`, `sonido/*.wav/.ogg`, `vista_previa/*`, `LEEME_TRANSICION.txt`, `reporte.json` | 6 |
| 7 | Reproducible scripts | Build scripts (drawing library, elements, animation, background, collection, transition), so you can regenerate when a text changes | 3–6 |
| 8 | QA report | `QA.md` with the §10 checklist filled (includes uniqueness) | 7 |
| 9 | Numbered ZIPs | Those in §8 | 7 |

---

## 13. Ready-to-paste prompts

### 13.1 MASTER PROMPT
```
Act as art director and technical producer for OBS overlays. Follow SPEC_OVERLAYS_OBS.en.md
(or Spanish SPEC_OVERLAYS_OBS.md) to the letter (phases 0–7, with an approval CHECKPOINT at the end of each phase).

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

### 13.2 Prompts per phase
- **F1, style bible:** "With images I1, I4, I5, and I9, propose a 6–10 color HEX palette with roles, a free font covering needed glyphs, a construction rule (grid or radii), and 3 title variants for '<NAME>'. Fill the §3 style bible template and generate a sample sheet. Do not copy Notblu’s palette or title treatment."
- **F2, branding:** "Create a 2560x1440 banner with everything important inside the centered 1546x423 safe zone, the mobile preview, an 800x800 avatar with circular preview from option <N>, and a description under 1000 characters focused on content, with no lore or fixed games."
- **F3, elements + composition:** "Explore composition from I1/I5/I9 and branding. Propose 2–3 distinct layout directions with mocks (Starting Soon, Game, Just Chatting). After I choose: write a unique layout.json, generate §4.3 roles as separate PNG RGBA (outer sizes free; §4.4 16:9 cutouts only if you use those frames), chat panel frame-only, reference scenes, and LEEME with positions from THIS layout.json. FORBIDDEN to use Appendix A coordinates. Uniqueness is a hard requirement."
- **F4, animation:** "Animate by code each approved PNG (§6): 3 s loops at 24 fps with changes every 2 frames and an opaque 12 s background. Export as lossless WebM VP9 yuva420p plus APNG. Verify alpha by decoding with libvpx-vp9, make preview GIFs and LEEME_ANIMADOS."
- **F5, collection:** "Generate the OBS 28+ JSON collection (1920x1080) with Starting Soon, Just Chatting, Game, BRB, and Ending scenes, paths to C:/<Channel>_OBS, Loop on, muted sources, Point filter, and one shared camera fitted to the layout.json cutout. Render scenes from the JSON and write LEEME_COLECCION (canvas first; chat/camera coords = this project’s layout.json)."
- **F6, transition:** "Create a 1920x1080 60 fps VP9 yuva420p stinger: main 1.5 s with cut at 750 ms, and fast 0.8 s with cut at 400 ms. Transparent at start and end, 100% opaque at the cut. Visual motif consistent with the style bible (do not clone Notblu’s stinger). Make with-original-audio and muted versions, a per-frame coverage report, and LEEME_TRANSICION."
- **F7, delivery:** "Package into the numbered ZIPs from §8, fill the §10 QA (include uniqueness checkboxes), and deliver the §12 manifesto."

---

## Appendix A: Resolved example (Notblu, @notblu14) — HISTORICAL REFERENCE ONLY

> # ⚠️ CRITICAL WARNING — DO NOT COPY
>
> This appendix documents **one** already-resolved channel (Notblu). Its coordinates, outer sizes, composition (central stack, chat on the right, camera bottom-left in Game, etc.), and measures like `~762x210` / `540x723 @ (1332,168)` are **values for that instance**.
>
> **NEVER** use them as a template, defaults, “recommended sizes,” or starting point for another channel.
> Reusing this geometry is an agent **failure** (§0.1).
> For any new project: invent the layout from I1/I5/I9 and the style bible; propose 2–3 directions; write your own `layout.json`.

| Field | Value (Notblu only — do not reuse) |
|---|---|
| Inputs | Channel screenshot, 1 concept art (pixel-art space / *space cowboy*), and 3 avatar options; **option 3** was chosen |
| Style | Pixel art with cyberpunk title (neon, circuits, scanlines, glitch) |
| Font | Press Start 2P (OFL, Google Fonts) |
| Grid | 1 cell = **3 px**, nearest-neighbor scaling; "Point" filter in OBS |
| Palette | Cyan `#40F6FF`, ice `#C8FCFF`, electric blue `#2878FF`, magenta `#FF2EC4`, pink `#FF78DC`, navy `#06081C`, deep violet `#140A3C`, muted blue `#283C6E`, purple `#78146E`, white `#FFFFFF` |
| Title | Banded gradient `#AAFFFF → #5AEBFF → #3CB4FF → #4678FF → #825AFF → #C846EB → #FF3CC8` |
| Banner | 2560x1440, 1546x423 safe zone (507–2053 × 508–931) |
| Avatar | 800x800 (option 3); 393x393 neon ring in the Ending scene |
| Description | Content-focused, no lore, no game names |
| Elements | ~25 PNGs with measures **specific to that layout** (not mandatory SPEC sizes) |
| Camera cutouts (technical contract met) | 384x216, 480x270, 1200x675 — *positions for that instance omitted on purpose so they are not inviting to clone* |
| Chat / scenes | Notblu’s own composition (central stack + chat right + cam bottom-left in Game). **Do not copy it.** |
| Animated | Lossless WebM VP9 yuva420p + APNG; 3 s / 24 fps / changes every 2 frames; avatar_anillo 4 s; one-shot alert |
| Background | Night sky, moon, and clouds; 12 s loop (288 frames at 24 fps) in MP4 H.264 (CRF 14) + WebM VP9 (CRF 18) |
| AI video | Grok Imagine was tried and **discarded**: no alpha, morphing, and loops that don’t close |
| Collection | `Notblu_coleccion_OBS.json`, paths `C:/Notblu_OBS/`, `looping` and `restart_on_activate` true, muted sources, Point filter, one shared "Cámara" |
| Transition | Main 1.5 s / cut **750 ms**; fast 0.8 s / cut **400 ms**; `_mudo` versions and original audio |
| Real issues | (1) Canvas ≠ 1920x1080 at import. (2) ZIP confusion → numbered names. (3) OBS in English → bilingual menus |
