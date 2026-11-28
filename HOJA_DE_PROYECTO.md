# CrazyHead · Hoja de proyecto

## Descripción
App web de Sampled Head (secuenciador/groovebox) en un solo `index.html`, con `sw.js` (caché `crazyhead-vX.YY`) y `manifest.webmanifest`.

## Arquitectura
- Todo en `index.html` (HTML, CSS y JS), publicado con GitHub Pages.
- `sw.js`: service worker con caché versionada. Al cambiar la versión se cambia también el nombre de la caché.
- También se distribuye dentro de las apps de Windows, Mac y Android de SH-apps, que se actualizan solas desde este repositorio.

## Reglas permanentes
- Cada actualización sube la versión (visible en la app y en el nombre de la caché).
- Entrega numerada en zip (`crazyhead-vX.zip` con carpeta `crazyhead-vX`), versión standalone de un solo archivo, informe de error `.txt` y esta hoja.
- **Zona segura:** nada interactivo en la franja de la hora, la batería o la cámara: la cabecera usa `env(safe-area-inset-top)`.

## GlitchHead (1.21)
Pestaña **Glitch**: el motor y el panel de GlitchHead (Shred · Haze · Split; repo `GlitchHead`) van copiados dentro de
`index.html` entre `<!--GH:BEGIN-->` y `<!--GH:END-->` con `node ../GlitchHead/tools/embed.js index.html` (no editar ahí).
El código propio de la pestaña está en el bloque `/* 1.21: pestaña GlitchHead */` al final.
- **En vivo** (`GHX.t`): Apagado, Máster (entre `eng.out` y el compresor) o un loop A–D (entre su `vol` y `eng.out`).
  Un solo AudioWorklet. Ajustes en `localStorage['crazyhead.gh']` (no van en los proyectos ni en las escenas).
- **Exportar**: `renderMix` está envuelta: con GlitchHead en el máster procesa la mezcla ya renderizada (va después del
  compresor, en vivo va antes); en un loop renderiza ese loop solo, lo procesa y lo suma al resto.
- **Procesar un loop**: procesa el audio de un loop (offline, en Worker) y lo carga en otro o en el mismo, manteniendo los
  cortes si se marca; «Deshacer» recupera lo que había (o vacía la ranura si estaba vacía). ⬇ WAV del loop procesado.

## Historial
- **1.25** (28-11-2026): icono nuevo con el estilo del logo de Sampled Head (círculo naranja con sombra, aparato en crema con huecos naranjas y pegatina negra inclinada con el nombre), como FuzzHead, GlitchHead y TapeHead. Archivos `icon-192.png`, `icon-512.png`, `icon-maskable-512.png`, `apple-touch-icon.png` y `favicon-32.png` (los iconos ya no van incrustados en base64 en el HTML ni en el manifiesto). Fuente: `gen.py` del chat (SVG → PNG con Chrome).
- **1.24** (24-11-2026): el «⋮» (Mezcla y exportar) pasa a ser el **menú común «☰ Menú»** de todas las apps (sh-core 1.5.0, `SH.ui.menu`): mismas secciones desplegables y en el mismo orden — Proyecto · Importar · Exportar · Otras apps · Copia de tus datos · Ajustes · Ayuda (guía e informe .txt) —; recuerda la sección abierta; los botones de siempre siguen haciendo lo mismo (el menú los pulsa); «⇄ Apps» va dentro (Otras apps, con el número de recibidos en el botón ☰). Master y Swing en Ajustes → Mezcla (`#mixkg`).
- **1.23** (24-11-2026): **arreglo**: la base `crazyhead` se abre sin número de versión fijo y crea los almacenes que falten (`openStores`); una copia .shdatos sumada en la app única de Android podía dejarla en otra versión y la app no podía abrirla. sh-core 1.4.1.
- **1.22** (24-11-2026): **más rápido con muchos cortes** (la onda, los tramos y los marcadores se dibujan una vez y se reutilizan —en el móvil emulado el fotograma pasó de ~33 a ~18 ms con 64 cortes en 4 loops—; la detección de golpes calcula el análisis una sola vez en vez de hasta 14; con más de 24 cortes solo se numera 1 de cada 4). **Región antes de trocear y Consolidar**: al cargar un audio (o con «✂ Región») se abre el editor del grabador con ese audio; «Consolidar y trocear» deja solo la parte elegida y vuelve a detectar golpes (opción «Preguntar al cargar»). **WAV exportados exactos**: mezcla y stems por pistas duran justo los compases elegidos; la cola se suma al principio (`loopCut`). **Enviar a SampledHead en las apps instaladas** (Android, Windows/Mac): por la bandeja común de sh-core en vez de `fromCrazyHead` (que no cruza entre apps instaladas); lo recibido del sampler conserva los cortes (`meta.cuts`).
- **1.21** (20-11-2026): pestaña **Glitch** con GlitchHead 1.0.0 (en vivo en máster o en un loop, procesar un loop y
  cargarlo en otro, incluido en WAV mezcla/por loop y ⏺ REC). Pestañas más compactas en móvil.
- **1.20** (19-11-2026): golpes fijos con **probabilidad** (100/75/50/25 %, estable por paso hasta regenerar: `D.pinProb`) y **por compás** (chips «Todos / 1…n»: `D.pinBars`, máscara de bits como el sidechain). `pinCell` decide; al cambiarlos se reaplican las columnas fijadas (`pinApply`). Recibe del sampler 1.68 los cortes del Chop al «Enviar pad a CrazyHead» (campo `cuts`, ya soportado).
- **1.19** (19-11-2026): conexión con todas las apps de Sampled Head (sh-core): botón «⇄ Apps» con «Recibido» y «Enviar a…»; en Windows/Mac usa la carpeta Documentos/SampledHead. Lo recibido entra en los loops A–D. En escritorio el modelo de stems va a la carpeta compartida. El sw ya no borra cachés de otras apps.
- **v1.18** (19-11-2026) — zona segura: la cabecera (y, donde las hay, las ventanas a pantalla completa y la barra inferior) deja libre la franja de la hora, la batería y la cámara del móvil con `env(safe-area-inset-*)`. Comprobado en Chrome simulando un móvil con 48 px arriba y 24 px abajo: ningún botón tapado. En ordenador no cambia nada.

## Límites conocidos
- GlitchHead (1.21): probado en Chrome headless (en vivo, procesar, deshacer, render con y sin GlitchHead); no oído en un
  móvil real. Sus ajustes no se guardan en los proyectos `.crazyhead`. En el máster, el WAV exportado aplica GlitchHead
  después del compresor (en vivo va antes): puede sonar algo distinto con niveles altos.
- Golpes fijos por probabilidad/compás (1.20): probada la lógica en Chrome headless; no oído en un móvil real.
- Esta hoja se creó en la v1.18; el historial anterior no está documentado aquí.
- Zona segura comprobada en un móvil simulado (Chrome); no en un móvil real.
