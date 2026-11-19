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

## Historial
- **1.20** (19-11-2026): golpes fijos con **probabilidad** (100/75/50/25 %, estable por paso hasta regenerar: `D.pinProb`) y **por compás** (chips «Todos / 1…n»: `D.pinBars`, máscara de bits como el sidechain). `pinCell` decide; al cambiarlos se reaplican las columnas fijadas (`pinApply`). Recibe del sampler 1.68 los cortes del Chop al «Enviar pad a CrazyHead» (campo `cuts`, ya soportado).
- **1.19** (19-11-2026): conexión con todas las apps de Sampled Head (sh-core): botón «⇄ Apps» con «Recibido» y «Enviar a…»; en Windows/Mac usa la carpeta Documentos/SampledHead. Lo recibido entra en los loops A–D. En escritorio el modelo de stems va a la carpeta compartida. El sw ya no borra cachés de otras apps.
- **v1.18** (19-11-2026) — zona segura: la cabecera (y, donde las hay, las ventanas a pantalla completa y la barra inferior) deja libre la franja de la hora, la batería y la cámara del móvil con `env(safe-area-inset-*)`. Comprobado en Chrome simulando un móvil con 48 px arriba y 24 px abajo: ningún botón tapado. En ordenador no cambia nada.

## Límites conocidos
- Golpes fijos por probabilidad/compás (1.20): probada la lógica en Chrome headless; no oído en un móvil real.
- Esta hoja se creó en la v1.18; el historial anterior no está documentado aquí.
- Zona segura comprobada en un móvil simulado (Chrome); no en un móvil real.
