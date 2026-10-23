# Prompt maestro (cópialo en tu IA)

Adjunta también [`docs/SPEC_OVERLAYS_OBS.md`](../docs/SPEC_OVERLAYS_OBS.md) y tus imágenes (I1–I9).

Existe un gemelo en inglés en [`docs/SPEC_OVERLAYS_OBS.en.md`](../docs/SPEC_OVERLAYS_OBS.en.md).

Rellena los `<…>` con los datos de **tu** canal.

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
