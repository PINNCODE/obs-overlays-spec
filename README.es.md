# OBS Overlays Spec (anti-clon)

[English → README.md](README.md)

**Playbook open-source** para que **cualquier** streamer arme un paquete de overlays OBS con una IA — sin terminar con el mismo HUD centrado de siempre.

Este repo **no** es un skin listo para Notblu. Es el SPEC + prompt para que tu agente invente layout, jerarquía y posiciones a partir de **tu** arte y assets.

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
| Apéndice Notblu | Solo ejemplo histórico con advertencia **NUNCA copiar** |
| QA | Checkboxes de unicidad + mocks previos |

Si dos canales hechos con este SPEC se ven hermanos en un screenshot, el agente **falló**.

## Cómo usarlo (3 pasos)

1. Baja o clona este repo.
2. Abre tu IA favorita y adjunta [`docs/SPEC_OVERLAYS_OBS.md`](docs/SPEC_OVERLAYS_OBS.md).
3. Pega el texto de [`prompts/PROMPT_MAESTRO.md`](prompts/PROMPT_MAESTRO.md) (o el inglés [`PROMPT_MASTER.md`](prompts/PROMPT_MASTER.md)), rellena tus datos y sube tus imágenes (I1–I9 del SPEC).

Empieza en **Fase 0**. No saltes a PNG sin elegir una dirección de layout.

## Contenido del repo

```
docs/SPEC_OVERLAYS_OBS.md     ← SPEC completa (fuente de verdad)
docs/CHANGELOG_anti_clon.md   ← qué cambió vs la versión “clonable”
prompts/PROMPT_MASTER.md      ← prompt en inglés listo para pegar
prompts/PROMPT_MAESTRO.md     ← prompt en español listo para pegar
examples/notblu/              ← capturas de UN ejemplo (no plantilla)
README.md                     ← README en inglés (principal en GitHub)
LICENSE                       ← MIT
```

## Para quién es

YouTube / Twitch / Kick creators que quieran un pack OBS serio (escenas Empezando, Charlando, Juego, Pausa, Final + transición) generado con IA, con identidad propia.

## Créditos

Creado a partir del trabajo de **Notblu** ([@notblu14](https://www.youtube.com/@notblu14)) con Grok Bot. El ejemplo visual en `examples/notblu/` es solo referencia histórica.

## Licencia

MIT — úsalo, forkéalo, mejóralo. Si publicas un pack hecho con este SPEC, un link al repo ayuda a que más gente no clone el mismo layout.
