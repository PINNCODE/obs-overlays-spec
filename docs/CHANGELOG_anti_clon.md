# CHANGELOG — SPEC overlays OBS (anti-clon)

Cambios pedidos por TRUJILLO: al aplicar la SPEC con otra IA el resultado salía casi idéntico a Notblu. Ahora el agente **debe jugar** con diseño y posicionamiento según arte/assets del proyecto.

## Qué cambió

- **§0.1 Principio de unicidad (anti-clon):** prohibido reusar coords/tamaños/composición del Apéndice A u otros canales; misma checklist de *tipos* OK, misma *geometría* = fallo; si dos proyectos se ven hermanos en screenshot, el agente falló.
- **§4.3:** la tabla de píxeles fijos (~762x210, etc.) pasó a **roles de elementos** con guías; tamaños exteriores libres según tipografía y arte.
- **§4.4 / §4.5:** solo contratos de **hueco** 16:9 (384×216 / 480×270 / 1200×675) y chat como marco; **posición libre** vía `layout.json` de este proyecto (sin coords Notblu).
- **§5:** eliminadas las recetas de coordenadas por escena; ahora **reglas de composición** + 2–3 direcciones de layout con mocks antes de bloquear.
- **Fase 3:** flujo explícito explorar assets → 2–3 mocks → elegir dirección → `layout.json` único → generar PNG.
- **§9 / LEEME:** chat y cámara deben citar `layout.json` de **ESTE** proyecto, no hardcodes de Notblu.
- **§10 QA:** checkboxes de unicidad (layout derivado del arte; distinto de Notblu; mocks previos).
- **§13 prompts:** unicidad como requisito duro; inventar layout; no usar Apéndice A; proponer 2–3 layouts.
- **Apéndice A:** advertencia grande — ejemplo histórico Notblu **solo**; no copiar coords/tamaños/composición; se omiten a propósito algunas posiciones para no invitar a clonar.

## Qué se mantuvo (técnico)

- Lienzo 1920×1080, PNG RGBA separados, sin textos que caduquen, animación por código + WebM VP9 yuva420p (+ APNG), stingers 1,5 s/750 ms y 0,8 s/400 ms, colección JSON, filtro Punto, loop, checkpoints, cuestionario, I1–I10, style bible, QA/LEEME/ZIPs/ffmpeg.
