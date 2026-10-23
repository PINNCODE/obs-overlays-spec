# SPEC: Paquete de overlays para OBS, de punta a punta (instrucciones para un agente de IA)

> Documento operativo. Léelo completo antes de empezar. Está escrito para que **otro agente de IA** lo ejecute con **cualquier streamer** y **cualquier estilo visual**.
> Los valores de Notblu (Apéndice A) son **un ejemplo resuelto de un canal concreto**. **Nunca copies su estilo, colores, textos, coordenadas, tamaños exteriores ni composición**, salvo que el usuario lo pida explícitamente para ese mismo canal.

---

## 0. Rol y objetivo

**Rol:** eres director de arte y productor técnico de overlays para streaming. Diseñas la identidad visual del canal y produces, **con código**, todos los recursos de OBS: estáticos, animados, la colección de escenas y la transición.

**Objetivo:** entregar un paquete que se instale en OBS en menos de 10 minutos y que no se rompa cuando cambie el contenido del canal. Incluye:

1. La identidad visual: guía de estilo (*style bible*) aprobada.
2. El branding del canal: banner, avatar y descripción.
3. Los elementos de OBS como **PNG separados y transparentes**.
4. Las versiones animadas en **WebM VP9 con alpha** (APNG como alternativa) y un fondo en bucle.
5. Una **colección de escenas en JSON**, lista para importar.
6. Una **transición stinger** con sonido y otra versión sin sonido.
7. Los archivos LEEME (instrucciones para el usuario final, en su idioma) y un informe de QA.

**Principios que no se negocian:**
- Nada de nombres de juegos, temporadas ni datos que caduquen dentro de los gráficos. El contenido del canal cambia.
- Cada elemento va en **su propio archivo**. Nunca entregues solo escenas "aplanadas".
- Las animaciones se generan **por código**. No uses video de IA para nada que necesite transparencia (ver §6.1).
- El lienzo de OBS es **1920x1080**. Todas las coordenadas se refieren a él.
- Pide aprobación en cada **CHECKPOINT** (🛑) antes de seguir.
- **Unicidad:** el layout de este canal debe inventarse a partir de sus assets; no reutilices geometría de otros proyectos (§0.1).

---

## 0.1 Principio de unicidad (anti-clon)

Este SPEC describe **qué tipos de elementos** producir y **qué contratos técnicos** cumplir. **No** describe un layout a clonar.

**Prohibido:**
- Reutilizar coordenadas, tamaños exteriores o composición del Apéndice A (Notblu) o de cualquier canal anterior como plantilla.
- Centrar por defecto el título + texto + barra + usuario como en Notblu, o poner siempre la cámara abajo-izquierda en Juego, solo porque "así salió el ejemplo".
- Tratar las medidas `~762x210`, `540x723 @ (1332,168)`, etc. del Apéndice A como tallas obligatorias.

**Obligatorio:**
- Inventar jerarquía, colocación y tamaños exteriores a partir de: concept art (I1), referencias SÍ/NO (I9), texturas (I5), logo/avatar y la style bible aprobada.
- La misma checklist de *tipos* de elementos está bien; la misma *geometría* es un fallo.
- Si dos proyectos hechos con este SPEC se verían como hermanos en una captura de pantalla (misma composición, mismos anclajes, mismos proporciones de títulos/marcos/chat), **el agente falló**.

En la Fase 3 debes **jugar con el diseño**: explorar, proponer 2–3 direcciones distintas y dejar que el usuario elija. El `layout.json` de cada canal es único.

---

## 1. Entradas requeridas

### 1.1 Cuestionario (hazlo al inicio; no avances sin las respuestas marcadas como obligatorias)

| # | Pregunta | Obligatoria | Notas |
|---|---|---|---|
| 1 | Nombre del canal, tal como debe verse (mayúsculas, acentos) | Sí | Revisa que la fuente elegida tenga todos los caracteres (Á, É, Í, Ó, Ú, Ñ, ¡, ¿). |
| 2 | Handle (@usuario) | Sí | Va en las etiquetas de usuario. |
| 3 | Plataforma(s): YouTube, Twitch, Kick… | Sí | Cambia el chat emergente (*popout*) y las alertas. |
| 4 | Tipo de contenido (de forma general: "juegos variados", "charlas", "arte") | Sí | Sirve para la descripción del canal. **No** pidas una lista fija de juegos. |
| 5 | Tono y personalidad (3 a 5 adjetivos) | Sí | Ejemplo: "nocturno, retro, relajado". |
| 6 | Estilo visual deseado (pixel art, flat, anime, minimalista, vaporwave…) | Sí | Confírmalo con el concept art. |
| 7 | Colores: favoritos y prohibidos | Sí | Si no hay, propón una paleta a partir del concept art. |
| 8 | Fuente(s) preferidas, o "elige tú" | No | Debe tener licencia libre (OFL o Google Fonts) y cubrir el español. |
| 9 | Idioma de los textos del overlay y del LEEME | Sí | |
| 10 | ¿Qué cosas cambian seguido? (juego, horario, metas, patrocinadores) | Sí | Todo lo que cambie va como **texto editable en OBS** o como fuente de navegador, nunca dentro de un PNG. |
| 11 | Escenas necesarias (lo mínimo: Empezando pronto, Charlando, Juego, Pausa, Final) | Sí | Pregunta por escenas extra. |
| 12 | Cámara: ¿usa? ¿en qué escenas? ¿tamaño preferido? | Sí | Define qué **roles** de marco se hacen (§4.3); la posición la decides en Fase 3. |
| 13 | Servicio de alertas (Streamlabs, StreamElements, nativo) | No | Define el formato de la alerta. |
| 14 | Sistema operativo, versión de OBS e **idioma de la interfaz de OBS** | Sí | Las instrucciones deben usar los nombres de menú de **su** idioma (ver §9). |
| 15 | Resolución actual del lienzo de OBS (Ajustes > Video) | Sí | Si no es 1920x1080, hay que cambiarla **antes** de importar. |
| 16 | ¿Quiere sonido en la transición? | No | Entrega siempre las dos versiones: con sonido y sin sonido. |
| 17 | ¿Tiene mascota o personaje? ¿Qué avatar usa? | No | |
| 18 | Ruta donde instalará los archivos (por defecto `C:\<Canal>_OBS`) | Sí | La ruta va escrita dentro del JSON de la colección. |

### 1.2 Lista obligatoria de imágenes (concept art y referencias)

Pídelas **antes** de diseñar. Sin las obligatorias no hay un concepto sólido. Estas imágenes **dirigen** la composición: no son decoración opcional.

| # | Imagen | Para qué sirve | Specs mínimas | Obligatoria |
|---|---|---|---|---|
| I1 | **Concept art o moodboard principal** (1 a 5 imágenes) | Fija el estilo, la atmósfera, la iluminación, la paleta **y los focos de atención** (dónde va el ojo → zona a dejar libre o a enmarcar). Puede servir de fondo base (si el usuario tiene los derechos). | ≥ 1920x1080 (ideal ≥ 2560x1440), PNG o JPG de buena calidad, sin marcas de agua | **Sí** |
| I2 | **Personaje o mascota** (si existe) | Para el avatar, el anillo del avatar y la escena Final. | ≥ 1024x1024, fondo liso o transparente, de frente o en 3/4 | Opcional (obligatoria si quiere personaje) |
| I3 | **Logo o tratamiento del título** (si existe) | Para el título del canal. Si no existe, lo diseñas tú. | Vector (SVG, PDF) o PNG ≥ 2000 px de ancho con transparencia | Opcional |
| I4 | **Muestra de paleta** (*swatch*) o lista HEX | Fija los colores exactos. Si no hay, la extraes de I1 y pides aprobación. | Imagen con bloques de color o lista `#RRGGBB` | Opcional (se genera en la Fase 1) |
| I5 | **Referencias de textura o fondo** | Para el patrón de fondo, el tipo de nubes, estrellas, ruido, *dithering*, scanlines. Influye en densidad y márgenes del HUD. | 2 o 3 imágenes, cualquier tamaño | Opcional, recomendada |
| I6 | **Opciones de avatar** (2 o 3) | Para que el usuario elija una. La elegida se usa en todo el paquete. | ≥ 800x800 cada una, en una sola lámina con etiquetas "OPCIÓN 1/2/3" | **Sí** (si no las tiene, genéralas y pide que elija) |
| I7 | **Captura del canal actual** | Para ver el banner y el avatar actuales, el recorte del avatar en círculo y lo que hay que conservar. | Captura completa de la página del canal en escritorio (y en móvil, si se puede) | **Sí** |
| I8 | **Foto de la cámara o del setup** | Para saber la relación de aspecto de la cámara (16:9 o 4:3), el encuadre y qué tipos de marco conviene. | Captura de la webcam tal como sale en OBS | **Sí** si usa cámara |
| I9 | **Referencias de "SÍ" y de "NO"** | Overlays que le gustan y overlays que no quiere (composición, densidad, chat a la izquierda/derecha, etc.). Evita iteraciones inútiles y clonar layouts que odia. | 2 o 3 de cada una, con una nota breve de por qué | **Sí** (al menos 1 de cada una) |
| I10 | Captura de su OBS actual (Ajustes > Video y la lista de escenas) | Para detectar la resolución del lienzo y el idioma de la interfaz. | Captura de pantalla | Recomendada |

> Si el usuario sube imágenes generadas por IA o de terceros, confirma que puede usarlas comercialmente en su canal.

---

## 2. Fases de trabajo

Cada fase tiene: pasos, criterios de aceptación (CA) y un 🛑 CHECKPOINT. **No avances de fase sin la aprobación explícita del usuario.**

### Fase 0: Recolección
1. Envía el cuestionario (§1.1) y la lista de imágenes (§1.2).
2. Revisa cada imagen: resolución, derechos y coherencia de estilo entre todas.
3. Haz un resumen de 5 líneas con lo que entendiste (estilo, tono, paleta tentativa, escenas, cámara) **y una nota de focos visuales de I1** (dónde empuja el arte la mirada).

**CA:** respuestas obligatorias completas, imágenes I1, I6, I7, I8 e I9 recibidas y el lienzo de OBS conocido.
🛑 **CHECKPOINT 0:** "¿Es correcto este resumen?"

### Fase 1: Identidad visual (style bible)
1. Extrae la paleta de I1 o I4: entre 6 y 10 colores con un rol para cada uno (fondo, acento 1, acento 2, texto, sombra, brillo).
2. Elige la fuente (de licencia libre, compatible con el español) y define su tamaño base.
3. Define la "regla de construcción": rejilla de píxel y escalado *nearest neighbor* para pixel art, o radios, grosores y sombras para un estilo vectorial.
4. Diseña el tratamiento del título (logo del canal) en 2 o 3 variantes.
5. Si hay opciones de avatar, el usuario elige una.
6. Llena la plantilla de la style bible (§3) y genera una lámina de muestra (paleta, fuente, título y un marco de ejemplo).

**CA:** la paleta tiene valores HEX, la fuente se ve con todas las tildes, el título se lee a 25 % de tamaño y hay un avatar elegido.
🛑 **CHECKPOINT 1:** aprobación de la style bible, el título y el avatar.

### Fase 2: Branding del canal
1. Banner de 2560x1440 con todo lo importante dentro de la **zona segura de 1546x423** centrada (x 507–2053, y 508–931).
2. Vista previa del recorte para móvil (1546x423) y para escritorio.
3. Avatar de 800x800 con vista previa del recorte circular.
4. Descripción del canal: habla del contenido y la personalidad. **Sin lore, sin nombres de juegos fijos**, y con el horario solo si el usuario lo pide.

**CA:** nada se corta en la vista móvil, el avatar se lee en círculo de 98 px y la descripción tiene menos de 1000 caracteres.
🛑 **CHECKPOINT 2:** aprobación del banner, el avatar y la descripción.

### Fase 3: Elementos estáticos para OBS (composición primero)
1. **Explora la composición desde los assets** (I1, I5, I9, logo, avatar, style bible): identifica focos, zonas densas/vacías y sesgos de las referencias SÍ/NO.
2. Propón **2–3 direcciones de layout distintas** (§5), con mocks compuestos de escena (Empezando + Juego + Charlando como mínimo). Ejemplos de direcciones: cámara asimétrica a la izquierda, pila vertical, HUD solo en esquinas, título centrado con acentos orbitales, chat a la izquierda vs a la derecha, variantes de esquina para el HUD de juego. Deben estar ancladas al moodboard, no a Notblu.
3. 🛑 En el checkpoint de esta fase el usuario **elige una dirección**. Solo entonces bloqueas el layout.
4. Escribe `layout.json` **único de este canal** (x, y, w, h de cada elemento en el lienzo 1920x1080) y el orden de capas.
5. Genera cada elemento de §4.3 como **PNG RGBA separado**, recortado a su contenido y con alpha 0 fuera del arte. Los tamaños *exteriores* crecen o encogen según tipografía y arte; **no** copies medidas del Apéndice A.
6. Los marcos de cámara (si se usan) llevan un **hueco 100 % transparente** con el contrato de aspecto de §4.4. El panel de chat es **solo un marco**; el chat real va encima como fuente de navegador.
7. Genera escenas completas solo como referencia visual, alineadas con `layout.json`.
8. Escribe el `LEEME.txt` con la lista de elementos, tamaños, **posiciones tomadas de `layout.json` de ESTE proyecto** (nunca coordenadas hardcodeadas de Notblu) y el orden de capas.

**CA:** pasa el QA del §10 para estáticos; no hay textos que caduquen; los huecos de cámara (si existen) cumplen el contrato 16:9; el layout se deriva del arte de este proyecto y **difiere en composición** del ejemplo Notblu.
🛑 **CHECKPOINT 3:** aprobación de la dirección de layout elegida y de las escenas de referencia (Empezando, Charlando, Juego, Pausa, Final).

### Fase 4: Animación
1. Anima cada elemento **por código** a partir de su versión estática (§6).
2. Exporta WebM VP9 con `yuva420p` y APNG como alternativa.
3. Haz un fondo animado en bucle perfecto (12 s recomendado), sin alpha (MP4 H.264 + WebM).
4. Verifica el alpha **decodificando** el WebM (no confíes en el encoder).
5. Haz GIFs de vista previa y un `LEEME_ANIMADOS.txt`.

**CA:** bucles sin salto (el primer cuadro es igual al siguiente del último), huecos con alpha 0 en todos los cuadros, mismas medidas que el estático (o coordenadas nuevas documentadas en `layout.json`) y nada de deformaciones.
🛑 **CHECKPOINT 4:** aprobación de los GIFs de vista previa.

### Fase 5: Colección de escenas para OBS
1. Genera `<Canal>_coleccion_OBS.json` (formato de colección de OBS 28+, `resolution` 1920x1080).
2. Rutas absolutas a `C:/<Canal>_OBS/…` (o la ruta que dio el usuario).
3. En cada fuente multimedia: `looping: true`, `restart_on_activate: true` y `muted: true`. Filtro de escalado "Punto" / "Point" en todo el pixel art.
4. Comparte "Cámara" entre escenas (una sola fuente, reutilizada) y ajústala al hueco con *bounds* según `layout.json`.
5. Renderiza cada escena desde el JSON (con valores de prueba para la cámara y el juego) para comprobar las posiciones.
6. Escribe el `LEEME_COLECCION.txt`: coordenadas de chat/cámara = **las de `layout.json` de ESTE proyecto**.

**CA:** el JSON se importa sin errores, las 5 escenas llenan el lienzo y no faltan archivos.
🛑 **CHECKPOINT 5:** el usuario importa la colección y confirma con una captura de pantalla.

### Fase 6: Transición stinger
1. Genera la transición según el §7: versión principal, versión rápida, con sonido y sin sonido.
2. Verifica cuadro por cuadro que el primer y el último cuadro tengan alpha 0 y que el cuadro del corte sea 100 % opaco.
3. Escribe el `LEEME_TRANSICION.txt` con el punto de transición en ms.

**CA:** se cumple el reporte de cobertura (§7) y el sonido no satura (pico ≤ −1 dBFS).
🛑 **CHECKPOINT 6:** el usuario la prueba en OBS.

### Fase 7: Entrega
Empaqueta según el manifiesto (§12) y entrega el informe de QA.

---

## 3. Plantilla de style bible (llénala en la Fase 1)

```yaml
canal: "<NOMBRE>"
handle: "@<handle>"
estilo: "<pixel art / flat / anime / ...>"
tono: ["<adj1>", "<adj2>", "<adj3>"]
paleta:                    # 6-10 colores, cada uno con un rol
  fondo_oscuro:  "#RRGGBB"
  fondo_medio:   "#RRGGBB"
  acento_1:      "#RRGGBB"   # bordes / neón principal
  acento_2:      "#RRGGBB"   # alertas / EN VIVO
  texto:         "#RRGGBB"
  brillo:        "#RRGGBB"
  sombra:        "#RRGGBB"
degradados: ["<lista de bandas para el título, si aplica>"]
fuente_titulos: "<nombre> (<licencia>)"
fuente_texto:   "<nombre> (<licencia>)"
construccion:
  rejilla_px: <n>          # pixel art: 1 celda = n px; vectorial: null
  escalado: "nearest"      # pixel art; "bilinear" para vector
  grosor_borde_px: <n>
  radios_px: <n o 0>
  sombra: "<offset, color>"
  efectos: ["scanlines", "dithering", "glow", ...]
titulo: "<descripción del tratamiento + archivo de referencia>"
avatar_elegido: "<opción N>"
textos_fijos: ["EMPEZANDO PRONTO", "VOLVEMOS EN UN MOMENTO", "GRACIAS POR VER", ...]
textos_variables_en_OBS: ["juego actual", "meta", ...]   # nunca dentro de PNG
prohibido: ["<colores/estilos/elementos que NO quiere>"]
direccion_layout: "<nombre de la dirección elegida en Fase 3>"  # único por canal
```

---

## 4. Specs técnicas exactas

### 4.1 Lienzo y branding

| Recurso | Tamaño | Formato | Notas |
|---|---|---|---|
| Lienzo de OBS (Base y Salida) | **1920x1080** | — | Configúralo **antes** de importar. Todas las coordenadas son para este lienzo. |
| Banner de YouTube | 2560x1440 | PNG + JPG (≤ 6 MB) | Zona segura de **1546x423** centrada (x 507–2053, y 508–931). |
| Vista previa móvil del banner | 1546x423 | PNG | Recorte de la zona segura, para revisión. |
| Avatar | 800x800 | PNG | Revisa que funcione en círculo (vista previa con máscara circular). |
| Descripción | ≤ 1000 caracteres | texto | Sobre el contenido, sin lore ni juegos fijos. |

### 4.2 Formatos, alpha, fps y bucles

| Tipo | Formato principal | Alternativa | Alpha | FPS | Duración |
|---|---|---|---|---|---|
| Elemento estático | PNG RGBA | — | Sí (alpha 0 fuera del arte) | — | — |
| Fondo estático | PNG RGB 1920x1080 | — | No | — | — |
| Elemento animado | **WebM VP9 `yuva420p`** | APNG (bucle infinito) | Sí | 24 | Bucle de 3 s (72 cuadros); 4 s para elementos que giran |
| Fondo animado | MP4 H.264 `yuv420p` | WebM VP9 `yuv420p` | No | 24 | **Bucle perfecto de 12 s** (288 cuadros) |
| Alerta | WebM VP9 con alpha | PNG | Sí | 24 | Se reproduce una vez (entrada y reposo), sin bucle |
| Transición stinger | **WebM VP9 `yuva420p`** con audio | versión `_mudo` | Sí | 60 | 1,5 s la principal y 0,8 s la rápida |
| Sonido | WAV 48 kHz | OGG | — | — | Original o con licencia |
| Vista previa | GIF / MP4 | — | — | — | Solo para revisión |

Comandos de referencia para ffmpeg:
```bash
# Elemento animado con alpha (sin pérdida para que el pixel art quede limpio)
ffmpeg -framerate 24 -i f_%04d.png -c:v libvpx-vp9 -pix_fmt yuva420p -auto-alt-ref 0 -lossless 1 -row-mt 1 out.webm
# Fondo (sin alpha)
ffmpeg -framerate 24 -i bg_%04d.png -c:v libx264 -preset slow -crf 14 -pix_fmt yuv420p -tune animation -movflags +faststart fondo.mp4
ffmpeg -framerate 24 -i bg_%04d.png -c:v libvpx-vp9 -crf 18 -b:v 0 -row-mt 1 -pix_fmt yuv420p fondo.webm
# Verificación de alpha: decodifica FORZANDO libvpx-vp9 (el decoder nativo descarta el alpha)
ffmpeg -c:v libvpx-vp9 -i out.webm d_%04d.png
```

### 4.3 Roles de elementos (mínimo recomendado) — tamaños exteriores libres

Produce estos **roles** cuando apliquen. El checklist de *tipos* es común; la **geometría no**. Los tamaños exteriores crecen o encogen para encajar tipografía, arte y la dirección de layout elegida. **No copies** medidas del estilo `~762x210` del Apéndice A.

| Rol / archivo típico | Guía (no talla fija) | Notas |
|---|---|---|
| `fondo.png` / `fondo_animado.mp4` | Exacto **1920x1080** | Fondo de Empezando, Pausa, Final, Charlando (y variantes si hace falta). |
| `fondo_<variante>.png` | 1920x1080 | Opcional. |
| `marco_hud_pantalla.png` | 1920x1080, centro transparente | Esquinas/reglas/bordes según el arte; no clones el HUD de Notblu. |
| `titulo_<canal>.png` | Encaja al logo/tipografía; legible a ~25 % | Jerarquía principal; tamaño y proporción salen del branding. |
| `etiqueta_<canal>_pequena.png` | Compacta para HUD de juego | Versión reducida del logo; no heredes proporciones de otro canal. |
| `etiqueta_transmision.png` | Franja o chip de esquina | Texto tipo "<CANAL> // TRANSMISIÓN" si el idioma lo pide. |
| `estado_conectando.png` / `estado_en_pausa.png` / `estado_fin.png` | Chips/barras de estado | Ancho según el texto; posición según la dirección de layout. |
| `texto_empezando_pronto.png`, `texto_volvemos.png`, `texto_gracias_por_ver.png` | Textos de escena | Jerarquía inventada (no apilar al centro por defecto). |
| `texto_suscribete.png` (o "SÍGUEME") | CTA | Peso visual coherente con I9. |
| `barra_carga.png` | Barra / progreso | Forma libre (segmentada, línea, etc.) según el estilo. |
| `usuario_<handle>.png` | Chip de handle | |
| `avatar_anillo.png` (u otro marco de avatar) | Según el arte | Solo si la escena Final / branding lo piden. |
| `marco_camara_pequeno.png` / `marco_camara.png` / `marco_camara_grande.png` | Exterior libre; **hueco** 16:9 exacto (§4.4) | Elige **qué** tipos de marco usar y **dónde** van; no es obligatorio usar los tres. |
| `etiqueta_en_vivo.png`, `etiqueta_cam.png` | Pestañas / badges | |
| `barra_alertas.png` | Guía con zona transparente | Ancho y anclaje según layout (arriba, lateral, etc.). |
| `panel_chat.png` | **Solo marco**; interior útil para el popout | Tamaño y lado (izq/der) según la dirección elegida. |
| `encabezado_charlando.png` | Encabezado de Charlando | Opcional según jerarquía. |
| `alerta_seguidor.png` | Espacio vacío documentado (x,y,w,h) para el nombre | |

### 4.4 Contratos de hueco de cámara (16:9, transparentes) — posición libre

Si usas un tipo de marco, el **hueco** debe ser un rectángulo 16:9 100 % transparente con estas medidas de contrato (el *exterior* del PNG y el *offset* del hueco dentro del archivo los defines tú, coherentes con el borde del arte):

| Tipo de marco (si se usa) | Tamaño del **hueco** (contrato) |
|---|---|
| Pequeño | **384×216** |
| Normal | **480×270** |
| Grande | **1200×675** |

- **Dónde** se coloca el marco en el lienzo lo decide la composición de la Fase 3 → `layout.json`.
- Posición de la cámara en el lienzo = posición del marco + offset del hueco dentro del PNG. La cámara va **debajo** del marco en la lista de fuentes.
- Si la webcam es 4:3, usa recorte (*crop*) o *bounds* "escalar hasta el exterior de los límites" con recorte; nunca la estires.
- No es obligatorio producir los tres tipos: produce los que el usuario y el layout necesiten.

### 4.5 Chat
- `panel_chat.png` es **solo el marco**. Tamaño exterior, lado (izquierda/derecha) y coordenadas salen de `layout.json` de **este** proyecto.
- Documenta el interior útil (x, y, w, h del área transparente) para la fuente de navegador.
- Fuente de navegador con el chat emergente de la plataforma y CSS `body { background: rgba(0,0,0,0); }`.
- En LEEME e importación: **usa las coords de `layout.json`**, no valores de Notblu.

---

## 5. Reglas de composición por escena (lienzo 1920x1080)

**No hay recetas de coordenadas fijas.** Las posiciones viven en `layout.json` único del canal. Documenta el orden de capas de **abajo → arriba** (en OBS, el último de la lista queda arriba en el panel de fuentes).

### 5.1 Antes de bloquear el layout
1. Propón **2–3 direcciones de layout distintas**, ancladas al moodboard (I1/I5/I9), por ejemplo:
   - Cámara asimétrica a la izquierda / a la derecha
   - Pila vertical (branding arriba, contenido abajo)
   - HUD solo en esquinas (centro limpio)
   - Título centrado con acentos orbitales
   - Chat a la izquierda vs chat a la derecha
   - Variantes de esquina para el HUD de juego (según normas de plataforma y del arte)
2. Muestra **mocks compuestos** de al menos Empezando, Juego y Charlando.
3. El usuario elige **una** dirección en el checkpoint. Solo entonces fijas `layout.json`.

### 5.2 Derivación desde el arte
- Localiza en I1 dónde va el ojo; deja esa zona más libre o enmárcala a propósito.
- Respeta sesgos de I9 (densidad, lado del chat, “mucho HUD” vs “minimal”).
- Márgenes seguros: ≥ **12 px** al borde en todos los elementos; nada crítico a menos de **36 px** del borde en escenas de pantalla completa (Empezando / Pausa / Final / Charlando).
- Legibilidad a tamaño de stream: títulos y CTAs deben leerse en una captura a ~25–50 % del lienzo.

### 5.3 Por escena (intención, no plantilla)
- **Juego:** deja el **centro jugable relativamente libre**. El HUD solo donde el arte y las normas de plataforma lo permiten; elige la esquina (o esquinas) según assets — **no** siempre cámara abajo-izquierda.
- **Empezando / Pausa / Final / Charlando:** inventa la jerarquía (título vs avatar vs CTAs vs estados) desde el branding. **No** apiles al centro por defecto “como Notblu”.
- **Charlando:** decide lado del chat, tamaño de cámara y encabezados según la dirección elegida; mantiene huecos 16:9 exactos si hay marco.
- Orden de capas: documentado bottom→top en LEEME y reflejado en el JSON.

### 5.4 Criterio de fallo
Si el mock final podría confundirse con el Apéndice A en una captura (mismos anclajes, misma pila central, mismo lado de chat y misma esquina de cámara), **rediseña** antes del checkpoint.

---

## 6. Animación y alpha

### 6.1 Por qué NO usar video de IA (Grok Imagine, Runway, Sora, etc.)
- **No tiene canal alpha.** Los overlays necesitan transparencia real; con un *chroma key* quedan halos y bordes sucios, sobre todo en pixel art y en el neón.
- **Los bucles no cierran:** el último cuadro no coincide con el primero y se nota el salto.
- **Hay deformación (*morphing*):** las letras, los marcos y el logo se derriten y cambian de forma entre cuadros. Eso destruye la marca y la rejilla de píxel.
- **No respeta medidas:** los huecos de cámara dejan de ser exactos.
→ El video de IA solo es aceptable como **inspiración de movimiento** o, como mucho, para un fondo opaco revisado. Nunca para elementos con transparencia.

### 6.2 Cómo animar (por código)
- Parte **del PNG estático aprobado**: la animación modifica capas (brillo, desplazamiento, máscaras), no redibuja la forma.
- Efectos válidos: parpadeo de neón en escalones, scanline que recorre el elemento, glitch de franjas con desfase cromático de 1 o 2 celdas, luz que recorre el borde, esquinas que pulsan, cursor parpadeante, puntos "..." que se llenan, barra que se llena, anillo que gira y paralaje del fondo.
- En pixel art, anima "en pasos": 24 fps de contenedor, pero el cambio ocurre cada 2 cuadros (sensación de 12 fps). Los movimientos van en múltiplos de la celda y nunca en subpíxeles.
- **Bucle perfecto:** todas las periodicidades deben dividir el número de cuadros (por ejemplo, 72 cuadros: periodos de 72, 36, 24, 18, 12…). En el fondo de 12 s (288 cuadros), cada capa en paralaje recorre un número entero de anchos de su mosaico.
- Los huecos (cámara, chat y alertas) se fuerzan a **alpha 0 en cada cuadro**.
- Mismas medidas que el estático. Si un elemento crece (cursor, puntos), mantén fijo el borde derecho o el centro y **documenta la nueva X en `layout.json`**.
- El fondo animado es opaco: MP4 y WebM sin alpha.
- La alerta es la única animación sin bucle: entrada y luego reposo.

### 6.3 Verificación obligatoria
Decodifica el WebM con `-c:v libvpx-vp9` y en cada cuadro comprueba lo siguiente: que el alpha del hueco sea 0, que el alpha fuera del arte sea 0, que el tamaño sea el correcto y que la diferencia entre el cuadro 0 y el siguiente del último sea igual a la que hay entre cuadros consecutivos. Guarda un `anim_report.json`.

---

## 7. Spec de la transición stinger

| Parámetro | Principal | Rápida |
|---|---|---|
| Archivo | `transicion_<canal>.webm` + `_mudo.webm` | `transicion_rapida.webm` + `_mudo.webm` |
| Resolución / fps | 1920x1080 / 60 | 1920x1080 / 60 |
| Duración | 1,5 s (90 cuadros) | 0,8 s (48 cuadros) |
| Códec | VP9 `yuva420p` + audio Opus | igual |
| Cuadro inicial / final | **100 % transparentes** | igual |
| Cobertura total | tramo continuo alrededor del corte (por ejemplo, 567–900 ms) | por ejemplo, 317–467 ms |
| **Punto de transición** | **750 ms** (cuadro 45) | **400 ms** (cuadro 24) |
| Contenido | Motivo visual coherente con la style bible (barrido, destellos, logo en el corte, etc.) | Variante más corta, sin logo si no cabe |
| Sonido | Whoosh + golpe en el corte, pico ≤ −1 dBFS, original | corto |

Reglas: el corte siempre cae dentro del tramo 100 % opaco (alpha 255 en todos los píxeles), nunca en el borde del tramo. No hace falta *track matte*. Entrega un PNG del cuadro del corte, un MP4 o GIF de vista previa y un `reporte.json` con la cobertura de cada cuadro.

Configuración en OBS: Transiciones de escena → "+" → Stinger → archivo `.webm` → Tipo de punto de transición: Tiempo (ms) → 750 (o 400) → Monitorización de audio a elección → Estilo de desvanecimiento del audio: "Desvanecer hasta el punto de transición". El campo "Duración" no afecta al stinger.

---

## 8. Convenciones de nombres y carpetas

- Nombres en minúsculas, `snake_case`, sin espacios, sin tildes y sin ñ (`pequeno`, no `pequeño`).
- El mismo nombre base para estático, animado y vista previa: `marco_camara.png` / `marco_camara.webm` / `marco_camara.apng` / `previews/marco_camara.gif`.
- Prefijos: `fondo_`, `marco_`, `etiqueta_`, `estado_`, `texto_`, `barra_`, `panel_`, `alerta_`, `transicion_`, `sonido_`.
- **Los ZIP deben decir qué contienen**, con número de orden:

```
<canal>_1_branding.zip                  banner, avatar, descripcion.txt
<canal>_2_elementos_png.zip             elementos/ + escenas_completas/ (referencia) + layout.json + LEEME.txt
<canal>_3_animados.zip                  *.webm, *.apng, fondo_animado.mp4/.webm, previews/, LEEME_ANIMADOS.txt
<canal>_4_COLECCION_OBS_IMPORTAR.zip    <Canal>_OBS/ (json + webm/mp4 que usa) + LEEME_COLECCION.txt   ← el que se importa
<canal>_5_transicion.zip                <Canal>_OBS/transiciones/ (webm, _mudo, sonido/, vista_previa/) + LEEME_TRANSICION.txt
```

Estructura de instalación en el PC del usuario (sin carpetas intermedias de más):
```
C:\<Canal>_OBS\
  <Canal>_coleccion_OBS.json
  LEEME_COLECCION.txt
  layout.json
  fondo_animado.mp4
  *.webm
  transiciones\
    transicion_<canal>.webm, transicion_<canal>_mudo.webm, transicion_rapida*.webm
    sonido\  vista_previa\
```

---

## 9. Configuración de OBS e importación (pasos para el usuario final)

> Escribe estos pasos en el idioma de la interfaz de **su** OBS. Pon entre paréntesis el nombre en el otro idioma, por ejemplo "Colección de escenas (Scene Collection) > Importar (Import)".
> **Coords de chat y cámara:** usa siempre las de `layout.json` / LEEME **de ESTE proyecto**. No hardcodees posiciones del Apéndice A (Notblu).

1. **PRIMERO, el lienzo:** Ajustes (Settings) > Video: Resolución base (lienzo) = **1920x1080** y Resolución de salida (escalada) = **1920x1080**. Aplicar. *(Si se importa con otro lienzo, las escenas no llenan la pantalla.)*
2. Descomprime `<canal>_4_COLECCION_OBS_IMPORTAR.zip` y deja la carpeta en `C:\<Canal>_OBS` (el JSON debe quedar directo dentro).
3. Colección de escenas (Scene Collection) > Importar (Import) > elige el `.json` > márcalo > Importar.
4. Colección de escenas > selecciona "<Canal>". Las escenas anteriores se conservan en su colección original.
5. Si OBS avisa de archivos faltantes, usa "Buscar en directorio" (Search Directory) y apunta a la carpeta.
6. Cámara: doble clic en "Cámara" > elige la webcam (es una sola fuente compartida entre Charlando y Juego). Ajústala al hueco según `layout.json`.
7. Captura de juego: modo pantalla completa por defecto. Si el juego va en ventana, cambia a capturar ventana específica.
8. Chat: agrega una fuente de navegador (Browser) con la URL del chat emergente, en las coordenadas e interior útil **documentados en `layout.json` / LEEME de este paquete**, con CSS de fondo transparente.
9. Alertas: fuente de navegador con la URL del widget, en la capa superior de la escena Juego (posición según layout). Sube `alerta_seguidor.webm` como video de la alerta.
10. Transición: §7.
11. Prueba cada escena en el modo estudio antes de salir en vivo.

Para añadir elementos sueltos a mano: "+" > Fuente multimedia (Media Source) > archivo local > marca Bucle (Loop). Luego clic derecho > Transformar > Editar transformación (alineación: arriba a la izquierda, escala 100 %). Por último, clic derecho > Filtro de escalado (Scale Filtering) > **Punto (Point)**.

Valores del JSON que debe generar el agente: `resolution: {x:1920, y:1080}`; fuentes `ffmpeg_source` con `is_local_file: true`, `local_file: "C:/<Canal>_OBS/<archivo>"`, `looping: true`, `restart_on_activate: true`, `close_when_inactive: false`, `muted: true`; en los elementos de escena, `scale_filter: "point"` para el pixel art; la cámara con `bounds_type` ajustado al hueco de **este** layout; y los dispositivos de audio del escritorio y del micrófono en "default".

---

## 10. Checklist de QA (llénala y entrégala)

**Unicidad / anti-clon**
- [ ] El layout se derivó del arte de **este** proyecto (I1, I5, I9, logo, avatar, style bible), no del Apéndice A.
- [ ] Se propusieron 2–3 direcciones de layout con mocks; el usuario eligió una antes de generar PNG finales.
- [ ] La composición **difiere** del ejemplo Notblu (no misma pila central, no mismos anclajes de cámara/chat por defecto).
- [ ] `layout.json` es único de este canal y LEEME/import usan esas coords (no hardcodes de Notblu).

**Estáticos**
- [ ] Todos son PNG RGBA (el fondo puede ser RGB) y están recortados a su contenido.
- [ ] Alpha 0 fuera del arte; no hay halos blancos ni negros en los bordes (revísalo sobre un fondo gris, uno verde y uno magenta).
- [ ] Si hay marcos de cámara: los huecos miden exactamente 384x216, 480x270 y/o 1200x675 (según tipos usados), con alpha 0.
- [ ] En pixel art, cada borde cae en la rejilla y no hay píxeles intermedios por el escalado.
- [ ] Las tildes y la Ñ se ven bien y no hay faltas de ortografía.
- [ ] No hay nombres de juegos ni datos que caduquen.
- [ ] Las escenas de referencia coinciden con `layout.json`.

**Animados**
- [ ] El WebM, al decodificarse, tiene alpha (se verifica con libvpx-vp9).
- [ ] Bucle perfecto; no se ve el salto en una reproducción de 3 ciclos seguidos.
- [ ] Los huecos tienen alpha 0 en todos los cuadros.
- [ ] Mismo tamaño que el estático, o la nueva X está documentada en `layout.json`.
- [ ] Hay APNG de respaldo y GIF de vista previa.
- [ ] El fondo dura 12 s, es opaco y tiene bucle perfecto, tanto en MP4 como en WebM.

**Colección**
- [ ] El JSON es válido, tiene `resolution` 1920x1080 y las rutas son correctas.
- [ ] Bucle activado, fuentes silenciadas y filtro Punto.
- [ ] La cámara es una sola fuente compartida, ajustada al hueco de este layout.
- [ ] Hay un render de cada escena desde el JSON.
- [ ] El ZIP de la colección tiene un nombre que deja claro que es el que se importa.

**Transición**
- [ ] El primer y el último cuadro tienen alpha 0.
- [ ] El tramo opaco es continuo y contiene el punto de corte (750 / 400 ms).
- [ ] Están las versiones con sonido y sin sonido; el audio no satura.

**Documentación**
- [ ] Los LEEME están en el idioma del usuario, con los nombres de menú de su OBS.
- [ ] El primer paso de la configuración es el lienzo de 1920x1080.
- [ ] LEEME_COLECCION cita `layout.json` de este proyecto para chat/cámara (sin coords de Notblu).

---

## 11. Errores comunes y soluciones

| Problema | Causa | Solución |
|---|---|---|
| El paquete se ve casi igual que Notblu / otro canal | Se copió geometría del Apéndice A o de un layout anterior | Vuelve a Fase 3: 2–3 direcciones nuevas desde I1/I9; unicidad es requisito (§0.1, §5). |
| Las escenas importadas no llenan la pantalla o quedan corridas | El lienzo de OBS no era 1920x1080 al importar | Ajustes > Video: Base y Salida en 1920x1080 **primero**; después borra la colección y vuelve a importar. |
| El usuario no sabe qué ZIP trae el JSON | ZIP con nombres ambiguos | Numera los ZIP y pon "COLECCION_OBS_IMPORTAR" en el nombre (§8). |
| Instrucciones que no coinciden con los menús | El OBS del usuario está en otro idioma (por ejemplo, inglés) | Pregunta el idioma (Q14) y escribe los nombres en los dos idiomas. |
| Archivos faltantes al importar | La carpeta no está en la ruta del JSON o hay una carpeta intermedia de más | Ruta exacta `C:\<Canal>_OBS\`, o "Buscar en directorio". |
| Pixel art borroso | Filtro de escalado bilineal o escala no entera | Filtro "Punto" y escalas al 100 %, 200 % o 300 %. |
| El WebM se ve con fondo negro | Exportado sin `yuva420p`, OBS antiguo, o se eligió el MP4 de vista previa | Reexporta con `-pix_fmt yuva420p -auto-alt-ref 0`, usa OBS 28+ y elige el `.webm`. |
| "El WebM no tiene alpha" al verificarlo | ffmpeg usó el decoder VP9 nativo | Decodifica con `-c:v libvpx-vp9` antes de `-i`. |
| Animación de IA con halos o que "se derrite" | Video de IA sin alpha y con *morphing* | Descártalo y anima por código (§6). |
| Salto visible en el bucle | Periodos que no dividen la cantidad de cuadros | Ajusta los periodos (§6.2) y verifica el cuadro N contra el cuadro 0. |
| El overlay muestra un juego que ya no se juega | Se escribió el juego en el PNG | Usa textos genéricos; lo variable va en una fuente de texto de OBS. |
| El chat tapa el marco o tiene fondo blanco | Fuente de navegador mal ubicada o sin CSS | Ponla en el interior del panel (coords de **este** `layout.json`) con `background: rgba(0,0,0,0)`. |
| La cámara se ve estirada | Webcam 4:3 metida en un hueco 16:9 | Usa *bounds* con recorte, no un estiramiento. |
| Se ve el cambio de escena antes de que la pantalla quede tapada | El punto de transición cae fuera del tramo opaco | Mide la cobertura y pon el corte en el centro del tramo. |
| El sonido de la transición sale o no sale en el directo | Monitorización de audio mal configurada | "Monitorizar y emitir", "Solo monitorizar", o usar `_mudo`. |
| Se corta el banner en el celular | Contenido fuera de la zona de 1546x423 | Revisa siempre la vista previa móvil. |
| LEEME con coords que no coinciden con el JSON | Se pegaron números del Apéndice A | Regenera LEEME desde `layout.json` de este proyecto. |

---

## 12. Manifiesto final de entregables

| # | Entregable | Archivos | Fase |
|---|---|---|---|
| 1 | Style bible | `style_bible.yaml` + `lamina_estilo.png` | 1 |
| 2 | Branding | `banner_2560x1440.png/.jpg`, `banner_movil_preview.png`, `avatar_800.png`, `avatar_circulo_preview.png`, `descripcion.txt` | 2 |
| 3 | Elementos PNG | `elementos/*.png`, `escenas_completas/*.png`, `layout.json` (único), mocks de direcciones (Fase 3), `LEEME.txt` | 3 |
| 4 | Animados | `*.webm` (VP9 alpha), `*.apng`, `fondo_animado.mp4/.webm` (12 s), `previews/*.gif`, `LEEME_ANIMADOS.txt`, `anim_report.json` | 4 |
| 5 | Colección de OBS | `<Canal>_OBS/<Canal>_coleccion_OBS.json` + los recursos que usa + `LEEME_COLECCION.txt` + renders de las escenas | 5 |
| 6 | Transición | `transicion_<canal>.webm`, `_mudo.webm`, `transicion_rapida.webm`, `_mudo.webm`, `sonido/*.wav/.ogg`, `vista_previa/*`, `LEEME_TRANSICION.txt`, `reporte.json` | 6 |
| 7 | Scripts reproducibles | Los scripts de construcción (biblioteca de dibujo, elementos, animación, fondo, colección, transición), para poder regenerar cuando cambie un texto | 3–6 |
| 8 | Informe de QA | `QA.md` con la checklist del §10 llena (incluye unicidad) | 7 |
| 9 | ZIPs numerados | Los de §8 | 7 |

---

## 13. Prompts listos para copiar y pegar

### 13.1 PROMPT MAESTRO
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

### 13.2 Prompts por fase
- **F1, style bible:** "Con las imágenes I1, I4, I5 e I9, propón una paleta de 6 a 10 colores en HEX con su rol, una fuente libre con tildes, una regla de construcción (rejilla o radios) y 3 variantes del título '<NOMBRE>'. Llena la plantilla de style bible del §3 y genera una lámina de muestra. No copies la paleta ni el tratamiento de Notblu."
- **F2, branding:** "Crea un banner de 2560x1440 con todo lo importante dentro de la zona segura de 1546x423 centrada, la vista previa móvil, un avatar de 800x800 con vista previa circular a partir de la opción <N>, y una descripción de menos de 1000 caracteres centrada en el contenido, sin lore ni juegos fijos."
- **F3, elementos + composición:** "Explora composición desde I1/I5/I9 y el branding. Propón 2–3 direcciones de layout distintas con mocks (Empezando, Juego, Charlando). Tras mi elección: escribe layout.json único, genera los roles del §4.3 como PNG RGBA separados (tamaños exteriores libres; huecos 16:9 del §4.4 solo si usas esos marcos), panel de chat solo-marco, escenas de referencia y LEEME con posiciones de ESTE layout.json. PROHIBIDO usar coordenadas del Apéndice A. Unicidad es requisito duro."
- **F4, animación:** "Anima por código cada PNG aprobado (§6): bucles de 3 s a 24 fps con cambios cada 2 cuadros y un fondo opaco de 12 s. Exporta en WebM VP9 yuva420p sin pérdida, más APNG. Verifica el alpha decodificando con libvpx-vp9, haz los GIFs de vista previa y el LEEME_ANIMADOS."
- **F5, colección:** "Genera la colección JSON de OBS 28+ (1920x1080) con las escenas Empezando pronto, Charlando, Juego, Pausa y Final, con rutas a C:/<Canal>_OBS, Bucle activado, fuentes silenciadas, filtro Punto y una sola cámara compartida ajustada al hueco de layout.json. Renderiza las escenas desde el JSON y escribe el LEEME_COLECCION (lienzo primero; coords de chat/cámara = layout.json de este proyecto)."
- **F6, transición:** "Crea un stinger de 1920x1080 a 60 fps en VP9 yuva420p: el principal de 1,5 s con el corte a 750 ms, y el rápido de 0,8 s con el corte a 400 ms. Deben ser transparentes al inicio y al final, y 100 % opacos en el corte. Motivo visual coherente con la style bible (no clones el stinger de Notblu). Haz versiones con sonido original y sin sonido, un reporte de cobertura por cuadro y el LEEME_TRANSICION."
- **F7, entrega:** "Empaqueta en los ZIPs numerados del §8, llena el QA del §10 (incluye checkboxes de unicidad) y entrega el manifiesto del §12."

---

## Apéndice A: Ejemplo resuelto (Notblu, @notblu14) — SOLO referencia histórica

> # ⚠️ ADVERTENCIA CRÍTICA — NO COPIAR
>
> Este apéndice documenta **un** canal ya resuelto (Notblu). Sus coordenadas, tamaños exteriores, composición (pila central, chat a la derecha, cámara abajo-izquierda en Juego, etc.) y medidas tipo `~762x210` / `540x723 @ (1332,168)` son **valores de esa instancia**.
>
> **NUNCA** los uses como plantilla, defaults, “tallas recomendadas” ni punto de partida para otro canal.
> Reutilizar esta geometría es un **fallo** del agente (§0.1).
> Para cualquier proyecto nuevo: inventa el layout desde I1/I5/I9 y la style bible; propón 2–3 direcciones; escribe un `layout.json` propio.

| Campo | Valor (solo Notblu — no reutilizar) |
|---|---|
| Entradas | Captura del canal, 1 concept art (espacio en pixel art / *space cowboy*) y 3 opciones de avatar; se eligió la **opción 3** |
| Estilo | Pixel art con título cyberpunk (neón, circuitos, scanlines, glitch) |
| Fuente | Press Start 2P (OFL, Google Fonts) |
| Rejilla | 1 celda = **3 px**, escalado nearest neighbor; filtro "Punto" en OBS |
| Paleta | Cian `#40F6FF`, hielo `#C8FCFF`, azul eléctrico `#2878FF`, magenta `#FF2EC4`, rosa `#FF78DC`, navy `#06081C`, violeta profundo `#140A3C`, azul apagado `#283C6E`, púrpura `#78146E`, blanco `#FFFFFF` |
| Título | Degradado por bandas `#AAFFFF → #5AEBFF → #3CB4FF → #4678FF → #825AFF → #C846EB → #FF3CC8` |
| Banner | 2560x1440, zona segura de 1546x423 (507–2053 × 508–931) |
| Avatar | 800x800 (opción 3); anillo neón de 393x393 en la escena Final |
| Descripción | Centrada en el contenido, sin lore, sin nombres de juegos |
| Elementos | ~25 PNG con medidas **propias de ese layout** (no son tallas obligatorias del SPEC) |
| Huecos de cámara (contrato técnico cumplido) | 384x216, 480x270, 1200x675 — *posiciones de esa instancia omitidas a propósito para no invitar a clonar* |
| Chat / escenas | Composición propia de Notblu (pila central + chat derecha + cam abajo-izq en Juego). **No la copies.** |
| Animados | WebM VP9 yuva420p sin pérdida + APNG; 3 s / 24 fps / cambios cada 2 cuadros; avatar_anillo de 4 s; alerta one-shot |
| Fondo | Cielo nocturno, luna y nubes; bucle de 12 s (288 cuadros a 24 fps) en MP4 H.264 (CRF 14) + WebM VP9 (CRF 18) |
| Video de IA | Se probó Grok Imagine y se **descartó**: sin alpha, con morphing y bucles que no cierran |
| Colección | `Notblu_coleccion_OBS.json`, rutas `C:/Notblu_OBS/`, `looping` y `restart_on_activate` en true, fuentes silenciadas, filtro Punto, una "Cámara" compartida |
| Transición | Principal 1,5 s / corte **750 ms**; rápida 0,8 s / corte **400 ms**; versiones `_mudo` y audio original |
| Problemas reales | (1) Lienzo ≠ 1920x1080 al importar. (2) Confusión de ZIPs → nombres numerados. (3) OBS en inglés → menús bilingües |
