# OBS Overlays Spec (anti-clon)

[English → README.md](README.md)

**Playbook open-source** para que **cualquier** streamer arme un paquete de overlays OBS con una IA — sin terminar con el mismo HUD centrado de siempre.

Este repo **no** es un skin listo para Notblu. Es el SPEC + prompt para que **tu** agente invente layout, jerarquía y posiciones a partir de **tu** arte y assets.

### Funciona con cualquier IA

ChatGPT, Claude, Gemini, Grok, Cursor, Copilot, modelos locales — **cualquier** asistente que pueda leer un Markdown largo y seguir pasos. Aquí **nada** está atado a un solo producto.

## Qué tienes que descargar

Solo necesitas **dos archivos** de este repo (o clona todo):

| Archivo | Qué es |
|---|---|
| [`docs/SPEC_OVERLAYS_OBS.md`](docs/SPEC_OVERLAYS_OBS.md) | Instrucciones completas que la IA debe seguir (**español** — principal) |
| [`docs/SPEC_OVERLAYS_OBS.en.md`](docs/SPEC_OVERLAYS_OBS.en.md) | Misma SPEC en **inglés** (gemelo) |
| [`prompts/PROMPT_MAESTRO.md`](prompts/PROMPT_MAESTRO.md) | Prompt corto que pegas primero (español). Gemelo en inglés: [`PROMPT_MASTER.md`](prompts/PROMPT_MASTER.md) |

Opcional: mira [`examples/notblu/`](examples/notblu/) como **un** resultado histórico — **no copies** ese layout.

## Prompt maestro (cópialo aquí mismo)

Rellena los `<…>` con los datos de **tu** canal, adjunta `SPEC_OVERLAYS_OBS.md` + tus imágenes (I1–I9), y pégalo en **cualquier** IA:

```
Actúa como director de arte y productor técnico de overlays para OBS. Sigue al pie de la letra el documento
SPEC_OVERLAYS_OBS.md (fases 0 a 7, con un CHECKPOINT de aprobación al final de cada fase).

REQUISITO DURO — UNICIDAD (anti-clon): inventa layout, jerarquía y colocación desde el concept art (I1),
referencias SÍ/NO (I9), texturas (I5), logo/avatar y la style bible. PROHIBIDO reutilizar coordenadas,
tamaños exteriores o composición del Apéndice A (Notblu) o de cualquier canal anterior. Misma checklist
de tipos de elementos = OK; misma geometría = FALLO. En Fase 3 propón 2–3 direcciones de layout con mocks;
el usuario elige una; luego escribe layout.json único y genera los PNG.

Canal: <NOMBRE> | Handle: @<handle> | Plataforma: <YouTube/Twitch/...> | Idioma: <es-MX>
Contenido (general, sin juegos fijos): <...> | Tono: <adj1, adj2, adj3> | Estilo: <...>
Colores deseados/prohibidos: <...> | Fuente: <... o "elige tú, licencia libre con tildes">
OBS: <versión>, interfaz en <idioma>, lienzo actual <WxH> | Ruta de instalación: C:\<Canal>_OBS
Adjunto: concept art (I1), personaje (I2, opcional), logo (I3, opcional), paleta (I4, opcional), texturas (I5),
opciones de avatar (I6), captura del canal (I7), foto de la cámara (I8), referencias SÍ/NO (I9).

Reglas técnicas: lienzo 1920x1080; cada elemento en PNG transparente separado; huecos de cámara (si se usan)
384x216 / 480x270 / 1200x675 como contrato de aspecto — posición y marco libre según layout; chat = solo marco
+ navegador; nada de textos que caduquen; animaciones por código en WebM VP9 yuva420p (APNG de respaldo),
sin video de IA; fondo en bucle de 12 s; stinger 1,5 s (corte 750 ms) y 0,8 s (corte 400 ms), con y sin sonido;
colección JSON importable (Bucle, silenciado, filtro Punto); ZIPs numerados; LEEME con lienzo 1920x1080 como
PRIMER paso y coords de chat/cámara tomadas de layout.json DE ESTE proyecto.
Empieza por la Fase 0: dame el cuestionario faltante y tu resumen (incluye focos visuales de I1).
```

El mismo texto está en [`prompts/PROMPT_MAESTRO.md`](prompts/PROMPT_MAESTRO.md) si prefieres un archivo aparte.

## Cómo usarlo (3 pasos)

1. Descarga / abre `docs/SPEC_OVERLAYS_OBS.md` y copia el prompt maestro de arriba (o de `prompts/PROMPT_MAESTRO.md`).
2. En **cualquier** IA: adjunta el SPEC, pega el prompt, rellena tus datos y sube tus refs (I1–I9).
3. Empieza en **Fase 0**. No saltes a PNG sin elegir una dirección de layout.

## Qué resuelve

- Overlays OBS 1920×1080 listos para importar (colección JSON)
- PNG separados con transparencia, animaciones WebM VP9 con alpha, stinger, branding
- **Unicidad obligatoria:** en Fase 3 la IA propone **2–3 direcciones de layout** con mocks; tú eliges; luego escribe un `layout.json` único. Misma lista de *tipos* de elementos ≠ misma geometría.

## Validación anti-clon (resumen)

| Qué | Estado |
|---|---|
| Coordenadas fijas tipo “título en (540,282)” como receta | **Eliminadas** del flujo operativo |
| Posición de cámara / chat | Libre vía `layout.json` del proyecto |
| Huecos de cámara 16:9 | Contrato técnico (aspecto), no plantilla de layout |
| Apéndice Notblu | Solo ejemplo histórico — **NUNCA copiar** |
| QA | Checkboxes de unicidad + mocks previos |

Si dos canales hechos con este SPEC se ven hermanos en un screenshot, el agente **falló**.

## Contenido del repo

```
docs/SPEC_OVERLAYS_OBS.md     ← SPEC completa (español — principal)
docs/SPEC_OVERLAYS_OBS.en.md  ← SPEC completa (inglés — gemelo)
docs/CHANGELOG_anti_clon.md   ← qué cambió vs la versión “clonable” (EN: CHANGELOG_anti_clon.en.md)
prompts/PROMPT_MASTER.md      ← prompt en inglés
prompts/PROMPT_MAESTRO.md     ← prompt en español (también copiado arriba)
examples/notblu/              ← capturas de UN ejemplo (no plantilla)
README.md                     ← README en inglés (principal en GitHub)
LICENSE                       ← MIT
```

> El SPEC en español sigue siendo el principal. Existe un gemelo en inglés: [`docs/SPEC_OVERLAYS_OBS.en.md`](docs/SPEC_OVERLAYS_OBS.en.md).

## Para quién es

YouTube / Twitch / Kick creators que quieran un pack OBS serio (Empezando, Charlando, Juego, Pausa, Final + transición) generado con IA, con identidad propia.


## Cómo agregarlo a OBS (cuando tu IA termine el pack)

Tu IA debería entregarte ZIPs + un LEEME / README de tu canal. Flujo resumido:

1. Pon el lienzo de OBS en **1920×1080** **antes** de importar: **Ajustes → Video → Resolución base (lienzo)** y **Resolución de salida (escalada)** ambas en `1920x1080`.
2. Descomprime el pack en la ruta que elegiste (ejemplo: `C:\TuCanal_OBS`). Respeta la estructura que diga el LEEME.
3. En OBS: **Colección de escenas → Importar** → elige el JSON del pack (ej. `*_coleccion_OBS.json`).
4. En fuentes multimedia que deban repetirse: activa **Bucle**. Para pixel art, filtro de escala **Punto** / Point.
5. Stinger (si viene): **Ajustes → Transiciones de escena → Stinger** → el WebM (VP9 + alpha). Punto de transición tipo **Tiempo (ms)** — cortes típicos: ~750 ms (stinger 1,5 s) o ~400 ms (rápido 0,8 s). Usa la versión muda si no quieres el sonido.
6. Chat: añade una fuente **Navegador** dentro del marco de chat; la posición sale de **tu** `layout.json` / LEEME — no copies coords de Notblu.
7. Cámara: mete tu captura en el hueco/marco de cámara en las escenas que la usen.

Si se ve estirado o borroso, revisa 1920×1080 y el filtro Punto antes de volver a importar.

## Créditos

Creado a partir del trabajo de **Notblu** ([@notblu14](https://www.youtube.com/@notblu14)) con Grok Bot. El ejemplo visual en `examples/notblu/` es solo referencia histórica.

## Licencia

MIT — úsalo, forkéalo, mejóralo. Si publicas un pack hecho con este SPEC, un link al repo ayuda a que más gente no clone el mismo layout.
