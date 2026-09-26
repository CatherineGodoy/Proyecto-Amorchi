# 💕 Proyecto Amorchi

Una página web romántica celebrando 14 años de amor, creada con mucho cariño y buenas prácticas de desarrollo.

## 📖 Historia

> *"De la fuente de Prontera al para siempre — cada nivel, cada quest, cada día a tu lado"*

Una historia de amor que comenzó en Ragnarok Online y se convirtió en una aventura para toda la vida.

## ✨ Características principales

| Característica | Descripción |
|----------------|-------------|
| 🎨 **Diseño romántico** | Paleta de colores rosa y dorado con gradientes suaves |
| 📸 **Galería de momentos** | Tarjetas con fotos y descripciones de momentos especiales |
| 🎵 **Reproductor de música** | Player con el tema de Prontera de Ragnarok Online |
| 💖 **Corazones flotantes** | Animación de emojis que ascienden por la pantalla con control de accesibilidad |
| 🔘 **Interruptor de animaciones** | Botón ON/OFF que decide él solo si la página se anima, y recuerda la elección |
| 📱 **Diseño responsive** | Se adapta a móviles, tabletas y escritorio |
| ♿ **Accesibilidad** | Cumple estándares WCAG; el sistema pone el valor inicial y el usuario lo puede pisar |
| 🎭 **Animaciones suaves** | Transiciones con IntersectionObserver para aparición progresiva |
| ⏱️ **Contador de tiempo juntos** | Reloj en vivo con días, horas, minutos y segundos desde el 15 de octubre de 2013 |
| 📊 **Barra de progreso de scroll** | Indicador visual en la parte superior que muestra cuánto se ha recorrido la página |
| 💞 **Separadores decorativos** | Líneas con corazón entre secciones para dar ritmo a la lectura |
| 🔍 **Lightbox de fotos** | Vista ampliada de las fotos de la galería con navegación y teclado completo |
| 🔊 **Control de volumen** | Slider de volumen y botón de silenciar que recuerda el último nivel |
| ✨ **Brillo de la carta** | Resplandor que sigue al cursor sobre la carta final |
| 🐑 **Ovejita viva** | La ovejita de cada tarjeta deambula sola, siempre en el mismo lado, y reacciona con una vueltita al hacer clic |
| 🏔️ **Fondo con parallax** | Las dos capas del fondo se deslizan a distinta velocidad al hacer scroll |
| 📅 **Línea de tiempo** | Una línea con nodos conecta los momentos de 2013, 2019 y 2025 |
| 💌 **Sello en la carta** | Lacre dibujado en SVG que remata la carta al pie |

## 🔧 Mejoras implementadas

### Fase 1 — Interacción y ritmo visual

- **Contador de tiempo juntos**: Cuatro tarjetas en vivo (días, horas, minutos, segundos) que calculan la diferencia real contra `TOGETHER_SINCE`. Se alinea al borde de cada segundo para evitar deriva y resincroniza al volver a la pestaña, ya que los timers se throtelan en segundo plano. Usa `font-variant-numeric: tabular-nums` para que el layout no salte cada segundo.
- **Barra de progreso de scroll**: Fija en la parte superior, se actualiza dentro de `requestAnimationFrame` con listeners `passive` para no bloquear el scroll. Es decorativa (`aria-hidden`) y desaparece al llegar al final.
- **Separadores decorativos**: Líneas con degradado y un corazón central entre las secciones. Comparten la clase `.section` para heredar la misma animación de aparición.
- **Movimiento reducido ampliado**: Además de las secciones, ahora desactiva corazones flotantes, el GIF de la ovejita, los efectos hover de tarjetas y botones, y la transición de la barra de scroll. Todo cuelga de una sola clase en `<html>` (ver "Interruptor general de animaciones").

### Fase 2 — Táctil, sonido y detalle

- **Lightbox de fotos**: Cada foto de la galería es ahora un `<button>` real, así que se abre con teclado y lector de pantalla. Al abrir, el foco salta al botón de cerrar, el fondo queda bloqueado y al salir el foco vuelve a la foto que lo abrió. `Escape` cierra, las flechas navegan con wrap-around, `Tab` queda atrapado dentro de los tres botones y un clic en el fondo también cierra. En móvil los botones de navegación bajan al pie de la foto.
- **Control de volumen**: Slider de volumen con relleno `--fill` y botón de silenciar que conmuta entre el último nivel y cero. El volumen se guarda en `lastVolume` solo cuando es distinto de cero, así que silenciar y volver nunca pierde el nivel anterior. `aria-pressed` y el texto accesible cambian con el estado.
- **Brillo de la carta**: Un resplandor radial sigue al cursor sobre la carta final usando las custom properties `--mx`/`--my`. El listener está siempre adjunto y consulta el interruptor al vuelo: si las animaciones están apagadas, el brillo queda en su posición estática.
- **`.gitignore`**: Ahora ignora `.atl/` para que los metadatos de las herramientas de asistencia no entren al repositorio.

### Fase 3 — Pulido visual

- **Parallax del fondo**: Las dos capas de fondo (`body::before` con el gradiente y `body::after` con el patrón de corazones) se desplazan a **distinta velocidad** según la variable `--scroll-p`, que va de 0 a 1 a lo largo del documento. La escriben el mismo `requestAnimationFrame` que ya movía la barra de scroll, así que **no hay ni un listener ni un rAF nuevos**. El gradiente recorre 160px (cielo lejano) y el patrón 460px (se aproxima): dos velocidades es lo que da profundidad. Cada capa tiene un `inset` negativo igual a su recorrido, de modo que nunca se descubre el borde, ni siquiera al 100% de scroll. Bajo `anim-off` vuelve a `transform: none` y sigue cubriendo todo.
- **Línea de tiempo**: Una línea de 2px con degradado recorre la columna de momentos y cada fila cuelga de ella con un nodo circular de 14px, pintado con el color de acento y un halo suave. El canaleta (`--tl-gutter`: 46px en escritorio, 30px en móvil) es el aire que deja la línea y sus nodos a la izquierda de las tarjetas. Los años **no se repiten** en la línea: ya están en el pie de cada tarjeta, así que la línea solo aporta la conexión 2013 → 2019 → 2025.
- **Sello en la carta**: Un lacre en SVG inline (dos círculos concéntricos, el segundo punteado, un corazón y el rótulo "14 AÑOS") remata la carta después del GIF. Va **en el flujo**, no superpuesto, así que nunca tapa texto; lleva un giro de -7° para que se lea estampado y no como un ícono. `role="img"` con `aria-label` para que el lector de pantalla lo anuncie.
- **Sin tecnología nueva**: todo lo de la Fase 3 es CSS y media línea de JS. Se evaluó `animation-timeline: scroll()` para hacer el parallax puramente en CSS, pero el comportamiento en el navegador de verificación no fue consistente, así que se prefirió el patrón de `requestAnimationFrame` que la página ya usaba y ya estaba probado.

### Fase 4 — Performance

- **Fotos en WebP**: las tres fotos de la galería pasaron de JPEG a WebP a calidad 82: 216 KB → 142 KB (**−34%**, 72 KB menos). El payload inicial de la página (sin contar la música) baja de 355 KB a 281 KB: **−21%**.
- **Sin salto de layout (CLS)**: los atributos `width`/`height` de las tres fotos decían `800×600`, que no coincidía con ninguna de las imágenes reales. Ahora declaran 960×720, 960×1280 y 520×1152, así que el navegador reserva la caja exacta antes de decodificar y la foto no mueve el contenido al cargar.
- **Fuentes por rango variable**: la URL de Google Fonts pide `ital,wght@0,400..700;1,400..700` y `wght@400..700` en lugar de pesos sueltos. Las dos familias ya se servían como **un solo woff2 variable** por subconjunto —las URLs de 400/500/700 eran idénticas—, así que el bytes descargado **no cambia**; lo que sí cambia es que bajan los bloques `@font-face` de 37 a 18 y, sobre todo, que los tres estilos que piden `font-weight: 600` dejan de quedar clampados a 700 y se renderizan en su peso real.
- **Safe areas**: `viewport-fit=cover` en el `<meta viewport>` permite que el fondo llegue al borde físico de la pantalla, y cuatro variables (`--sat`/`--sar`/`--sab`/`--sal`, todas con fallback a `0px`) empujan los controles interactivos fuera de la muesca y de la barra de inicio: barra de scroll, botón flotante de animaciones, pie de página, cerrar y navegar del lightbox, y el padding horizontal de `.page`. En pantallas sin muesca todo evalúa a 0 y el layout es idéntico al anterior.
- **Lo que queda igual, a propósito**: los GIFs (`bubu-dudu.gif`, `ovejita.gif`) no se convierten porque convertir un GIF a WebP con canvas descartaría la animación y dejaría un fotograma fijo, y no hay herramienta de conversión animada disponible. La música (`prontera-theme.mp3`, 8.9 MB) usa `preload="metadata"`, así que no entra en el primer render.
- **Medición honesta**: se comprobó contra la API de Google Fonts que los pesos sueltos y los rangos apuntan a los **mismos archivos woff2**, por lo que el ahorro real de T11 es de CSS y de correctitud de peso, **no de bytes transferidos**.

### Pulido — Ovejita de la galería

- **Deambula sola**: Cada ovejita recorre una elipse de 24×7px descentrada hacia arriba (`@keyframes ovejaVuelta`, 4s en bucle) en lugar de quedarse quieta. El recorrido cabe entero en el hueco de la fila (24px en escritorio) y en móvil deja más de 4px sobre la foto, así que nunca la toca.
- **Reacciona al clic**: Gira 360° y da un salto hacia arriba (`@keyframes ovejaSalto`, 700ms). Termina exactamente donde arranca `ovejaVuelta`, así que la vuelta al sitio continúa sin salto. Solo trepa, y arriba hay 40px libres: el título y el gap entre filas.
- **Sin interferencia con la foto**: Se quitó `pointer-events: none` para poder clickearla. Se verificó en escritorio, tablet y celular que su rectángulo nunca se cruza con ninguna foto, el título ni las tarjetas, así que el lightbox sigue recibiendo su propio clic.
- **Fallo seguro**: si `animationend` no llega (pestaña en background o movimiento reducido activado a mitad de vuelta), un `setTimeout` de 750ms la suelta para que no se quede clavada.
- **Apagada no promete nada**: sin animación no hay reacción posible, así que el cursor queda en `default` para no prometer un clic que no haría nada. Los listeners siguen adjuntos, así que al encender el interruptor la ovejita vuelve a responder sin recargar.
- **Las tres filas, idénticas**: se quitó el `flex-direction: row-reverse` de la fila par. Antes esa regla ponía la ovejita de la fila 2019 a la **derecha** (desentonando con las otras dos y con la línea de tiempo) y además desplazaba su tarjeta 89px respecto a las demás. Ahora las tres filas dan exactamente los mismos valores: ovejita en el mismo `x`, tarjeta en el mismo eje y con el mismo ancho, y nodo de la línea en el mismo punto. En móvil el layout es columna: la ovejita queda centrada arriba de la foto, igual en las tres.
- **Nodo exacto sobre la línea**: el `left` del nodo pasó de `-7px` a `-6px` respecto a la canaleta, porque el centro del nodo (`left + 7`) tiene que caer sobre el centro de la línea (`gutter/2 + 1`, ya que la línea mide 2px). Con `-7px` el nodo quedaba 1px desviado — imperceptible a simple vista, pero un eje que no es un eje.

### Interruptor general de animaciones

El botón que antes solo apagaba los corazones ahora decide **si la página se anima en general**, y resuelve un bug real: decía "Corazones: ON" mientras `prefers-reduced-motion` escondía todo con `display: none !important`, o sea que mostraba un estado falso.

- **Una sola fuente de verdad**: una clase `anim-off` en `<html>`. Todas las reglas de reposo son `html.anim-off ...` con `!important` — corazones, GIF de la ovejita, hovers, brillo de la carta, barra de scroll, transiciones de aparición y `opacity: 1` fijo en `.section` para que **el contenido nunca quede oculto**.
- **Se decide antes del primer pintado**: un script de 11 líneas en el `<head>` aplica la clase sin parpadeo, con esta prioridad: `localStorage('amorchi-animaciones')` → `prefers-reduced-motion` del sistema → encendido.
- **El usuario le gana al sistema**: al hacer clic se cambia la clase y se guarda en `localStorage`, así que la elección persiste aunque se abra el archivo directo (`file://` guarda y lee sin problema).
- **Botón honesto**: texto estable `Animaciones: ON/OFF` con `aria-pressed` llevando el estado (no el `aria-label`, que así no cambia y no hace ruido en lectores de pantalla).
- **Reacción en vivo**: `spawnHeart`/`startHearts`, el brillo de la carta y el clic de la ovejita consultan `animacionesEncendidas()` al momento de actuar, así que no hace falta recargar para que surta efecto.

### Corrección de errores críticos

- **Bug del reproductor de audio**: Se corrigió un error donde `audio` se utilizaba antes de ser declarado, lo que impedía la reproducción automática al iniciar la experiencia.

### Funcionalidad de corazones flotantes

- **Corazones por defecto**: Aparecen automáticamente al cargar, salvo que el usuario ya haya decidido lo contrario.
- **Se apagan con todo lo demás**: dejan de nacer en cuanto el interruptor está en OFF.
- **Limpieza automática**: Al desactivar, se eliminan todos los corazones existentes de la pantalla.
- **Elección guardada**: la preferencia vive en `localStorage('amorchi-animaciones')`; si no hay ninguna, manda `prefers-reduced-motion` del sistema.

### Accesibilidad

- **`prefers-reduced-motion`**: Es la **semilla** del interruptor: si el sistema pide menos movimiento, la página arranca apagada. El usuario puede encenderla igual desde el botón.
- **Contenido nunca oculto**: con `anim-off`, `.section` queda en `opacity: 1` aunque el `IntersectionObserver` nunca dispare — nadie se queda sin poder leer la página.
- **Dimensiones en imágenes**: Todas las imágenes declaran `width` y `height` para prevenir layout shift (CLS).
- **Contraste mejorado**: Color de texto secundario mejorado de `#55424d` a `#4a3644` para un ratio de contraste de 7:1.
- **Navegación por teclado**: El reproductor de música y el interruptor de animaciones son completamente navegables con teclado.
- **Estados ARIA**: El interruptor usa `aria-pressed` y un `aria-label` estable ("Animaciones") para no recitar el estado completo en cada cambio.

### Código limpio

- **Constantes con nombre**: Valores mágicos reemplazados por constantes descriptivas (`FLOAT_ITEM_LIFETIME`, `FLOAT_ITEM_INTERVAL`).
- **Español neutro**: Mensajes de error y textos de interfaz en español neutro, sin regionalismos.
- **Organización del código**: Variables del DOM y reproductor declaradas antes de ser utilizadas.

## 🛠️ Tecnologías

| Tecnología | Uso |
|------------|-----|
| HTML5 | Estructura semántica con `section`, `article`, `aria-label` |
| CSS3 | Custom Properties, Flexbox, `clamp()`, `@keyframes` |
| JavaScript vanilla | Sin dependencias externas, 100% nativo |
| Google Fonts | Cormorant Garamond, Great Vibes, DM Sans |

## 🚀 Cómo usar

1. Clonar el repositorio:
   ```bash
   git clone https://github.com/CatherineGodoy/Proyecto-Amorchi.git
   ```

2. Abrir `index.html` en tu navegador

3. Hacer clic en "Comenzar nuestra historia" para iniciar la experiencia

4. Usar el botón de la esquina inferior derecha (`Animaciones: ON/OFF`) para encender o apagar todo lo animado — la elección queda guardada

> Si la página te arranca sin animaciones es porque Windows tiene "Efectos de animaciones" apagados (**Configuración → Accesibilidad → Efectos visuales**). No hace falta cambiarlo: el botón la enciende por su cuenta.

## 📁 Estructura del proyecto

```
├── index.html                    # Página principal (HTML + CSS + JS)
├── README.md                     # Documentación del proyecto
├── .gitignore                    # Archivos ignorados por Git
├── prontera-theme.mp3            # Música de fondo (Tema de Prontera)
├── ovejita.gif                   # GIF decorativo para las tarjetas
├── bubu-dudu.gif                 # GIF de la pareja
├── foto1-primer-encuentro.webp    # Foto del primer encuentro (2013, WebP)
├── foto2-momentos-felices.webp    # Foto de momentos felices (2019, WebP)
├── foto3-siempre-juntos.webp      # Foto de siempre juntos (2025, WebP)
```

## 🎨 Personalización

### Cambiar la música

Edita la variable `MUSIC_CONFIG` en el `<script>`:

```javascript
const MUSIC_CONFIG = {
  src: './tu-cancion.mp3',        // Ruta del archivo de audio
  title: 'Título de la canción',  // Nombre de la canción
  artist: 'Artista'               // Nombre del artista
};
```

### Cambiar la fecha del contador

Edita la constante `TOGETHER_SINCE` en el `<script>`. El mes se indexa **desde 0**: enero es `0`, octubre es `9`.

```javascript
// Formato: new Date(año, mes - 1, día, hora, minuto, segundo)
const TOGETHER_SINCE = new Date(2013, 9, 15, 0, 0, 0); // 15 de octubre de 2013
```

El texto visible `Desde el 15 de octubre de 2013` está en el HTML y hay que actualizarlo a mano si cambia la fecha.

### Cambiar los colores

Modifica las variables CSS en `:root`:

```css
:root {
  --accent: #d63384;        /* Color principal (rosa) */
  --accent-dark: #9b1d55;   /* Versión oscura del acento */
  --accent-light: #fce7f3;  /* Versión clara del acento */
  --bg: #fdf6f9;            /* Color de fondo */
  --ink: #20171c;           /* Color del texto principal */
  --ink-muted: #4a3644;     /* Color del texto secundario */
}
```

### Personalizar los corazones

Para cambiar los emojis de los corazones flotantes, modifica el array `heartsItems` en el JavaScript:

```javascript
const heartsItems = ['❤️','💖','💕','💗','💓','💞'];
// Agrega o elimina emojis según tu preferencia
```

Para ajustar la velocidad o frecuencia de los corazones:

```javascript
const FLOAT_ITEM_LIFETIME = 25000;  // Tiempo de vida en milisegundos
const FLOAT_ITEM_INTERVAL = 1200;   // Intervalo entre corazones en milisegundos
```

### Agregar más momentos

Duplica un bloque `.memory-row` en el HTML y modifica:
- La imagen (`src` y `alt`)
- El título (`h3`)
- La descripción (`p`)
- El año (`memory-footer-year`)

No inviertas el orden de los hijos: la ovejita va **primero** y la tarjeta después, en las tres filas. El patrón alternado (`row-reverse`) se eliminó justamente porque descolocaba la ovejita y desplazaba esa tarjeta respecto a las demás.

## 📋 Changelog

### v1.6.0 (2026-09-26)

**Fase 4 — Performance:**
- 🖼️ Las tres fotos pasan de JPEG a **WebP** (calidad 82): 216 KB → 142 KB, **−34%**; payload inicial sin música 355 KB → 281 KB, **−21%**. Se borraron los JPEG originales
- 📐 Los `width`/`height` de las fotos pasan de `800×600` (falsos) a las dimensiones reales 960×720, 960×1280 y 520×1152 → cero layout shift al cargar
- 🔤 La URL de fuentes pasa a rangos variables (`0,400..700;1,400..700` y `400..700`): 37 bloques `@font-face` → 18, y los estilos que piden `600` dejan de quedar clampados a 700. Los bytes descargados no cambian, verificado contra la API de Google Fonts
- 📱 `viewport-fit=cover` + variables `--sat/--sar/--sab/--sal`: barra de scroll, botón flotante, pie, botones del lightbox y padding de `.page` quedan fuera de la muesca y de la barra de inicio; en pantallas sin muesca todo evalúa a 0

**Verificación:**
- ✅ **241 PASS / 0 FAIL** en viewports **exactos** de 1280 / 768 / 390 px (25 de Fase 4 + 40 de Fase 3 + 15/16 de la ovejita)
- ✅ Sin overflow horizontal en 390px: `scrollWidth === clientWidth` y 0 elementos fuera del viewport, idéntico antes y después del cambio
- ✅ Las tres WebP cargan con `naturalWidth/naturalHeight` exactos
- ✅ `node --check` OK en los 2 bloques, llaves CSS 154/154, etiquetas balanceadas (10/10 botones, 8 `<img>`)

### v1.5.1 (2026-09-25)

**Correcciones:**
- 🐑 Las tres ovejitas quedan **siempre en el mismo lado** (izquierda, junto a la línea de tiempo): se quitó el `flex-direction: row-reverse` de la fila 2019, que las ponía a la derecha —desentonando con las otras y con la línea— y además desplazaba esa tarjeta 89px respecto a las demás
- 📍 El nodo de la línea de tiempo se corrió 1px (`-7px` → `-6px`) para que su centro caiga exactamente sobre el centro de la línea, que mide 2px

**Verificación:**
- ✅ 46 aserciones, **0 fallos**, en 1280 / 768 / 390 px: las tres filas con `0px` de diferencia en el `x` de la ovejita, el eje y el ancho de la tarjeta; los tres nodos centrados sobre la línea en `0,0,0px`
- ✅ Sin solapes: la ovejita no toca el nodo de la línea, la foto ni el título; en móvil queda centrada arriba de la foto
- ✅ Regresión de Fase 3 completa: 120 PASS / 0 FAIL en los tres anchos
- ✅ `node --check` OK en los 2 bloques, llaves CSS 154/154, etiquetas balanceadas

### v1.5.0 (2026-09-25)

**Nuevas funcionalidades:**
- 🏔️ Parallax en las dos capas de fondo, a velocidad distinta entre sí, sin listeners nuevos
- 📅 Línea de tiempo con degradado y nodos que conecta los momentos de 2013, 2019 y 2025
- 💌 Sello tipo lacre en SVG inline al pie de la carta, con corazón y rótulo "14 AÑOS"

**Mejoras:**
- 📐 El canaleta de la línea de tiempo (`--tl-gutter`) se ajusta solo en móvil, de 46px a 30px
- 🧊 El parallax se congela con el interruptor general de animaciones y el fondo sigue cubriendo el viewport
- ♿ El sello es un `role="img"` con `aria-label`; el SVG sigue siendo texto seleccionable

**Verificación:**
- ✅ 120 aserciones funcionales, **0 fallos**, en 1280 / 768 / 390 px
- ✅ Parallax: a mitad de scroll `--scroll-p = 0.5001` → capas en `-230px` y `-80px` (las dos, exactas)
- ✅ La línea no pisa ninguna tarjeta y sus nodos caen al píxel sobre ella (250/250 y 39/39)
- ✅ El sello no tapa ni la firma ni el GIF, y su dibujo y su texto caben en el `viewBox`
- ✅ Regresión: contador, barra de scroll, lightbox, ovejita, corazones e interruptor — todo PASS
- ✅ `node --check` en los 2 bloques, llaves CSS 155/155, etiquetas balanceadas

### v1.4.2 (2026-09-25)

**Nuevo comportamiento:**
- 🔘 El botón de accesibilidad pasó de "Corazones" a **interruptor general de animaciones** (`Animaciones: ON/OFF`) — ya no miente: antes decía "ON" mientras `prefers-reduced-motion` escondía todo

**Mejoras:**
- 🌱 Una clase `anim-off` en `<html>` reemplaza al bloque `@media (prefers-reduced-motion: reduce)`, con `html.anim-off .section{opacity:1!important}` de red de seguridad
- ⚡ Decisión en un script del `<head>`: `localStorage` → `prefers-reduced-motion` → encendido, aplicada sin parpadeo
- 🧠 Elección persistente en `localStorage('amorchi-animaciones')` — el usuario le gana al sistema y se acuerda aunque se abra `file://`
- 🔁 Listeners de brillo, corazones y clic de la ovejita siempre adjuntos, consultando `animacionesEncendidas()` al vuelo

**Verificación:**
- ✅ 35 aserciones funcionales en navegador, **0 fallos**, en 2 corridas con perfil compartido (apagado por defecto → encendido → persistencia → elección guardada le gana al sistema)
- ✅ Invariante crítico: 8/8 secciones en `opacity 1` **sin `.visible`** con todo apagado
- ✅ Recorrido de la ovejita medido 24×7px y `playState: running`
- ✅ `node --check` sobre los 2 bloques de JS, llaves CSS 149/149, etiquetas balanceadas, 0 residuos del refactor

### v1.4.1 (2026-09-25)

**Nuevas funcionalidades:**
- 🐑 Ovejita de la galería que deambula sola en una elipse de 24×7px y reacciona al clic con una vueltita de 360° y salto

**Mejoras:**
- 🎯 `cursor: pointer` como señal de que la ovejita es clicable, y `cursor: default` bajo `prefers-reduced-motion`
- 🛡️ Respaldo por `setTimeout` para que la ovejita nunca quede trabada en la animación de clic

**Verificación:**
- ✅ 39 aserciones funcionales en navegador (escritorio 1280, tablet 768 y celular 390): movimiento autónomo, rango 23×7px, clic → `ovejaSalto` → vuelta a `ovejaVuelta`, y cero colisiones con foto, título o tarjeta
- ✅ `node --check` sobre el JS extraído, etiquetas balanceadas y llaves CSS 150/150

### v1.4.0 (2026-09-25)

**Nuevas funcionalidades:**
- 🔍 Lightbox de fotos con navegación por teclado, trampa de foco, bloqueo del scroll y devolución del foco al abrir
- 🔊 Slider de volumen con relleno visual y botón de silenciar que recuerda el último nivel
- ✨ Brillo radial en la carta final que sigue al cursor

**Mejoras:**
- ♿ Las fotos de la galería pasaron de `<div>` a `<button>`, lo que las hace accionables con teclado y visibles para lectores de pantalla
- 🎯 `aria-label`, `aria-pressed` y `title` del control de volumen actualizados según el estado
- 🧰 `.gitignore` ahora excluye `.atl/`

**Verificación:**
- ✅ 26 aserciones funcionales en navegador (apertura, foco, teclado, wrap-around, volumen, glow)
- ✅ `node --check` sobre el JS extraído, etiquetas balanceadas y llaves CSS 136/136

### v1.3.0 (2026-09-24)

**Nuevas funcionalidades:**
- ⏱️ Contador en vivo de tiempo juntos (días, horas, minutos, segundos) desde el 15/10/2013
- 📊 Barra de progreso de scroll fija en la parte superior
- 💞 Separadores decorativos con corazón entre secciones

**Mejoras:**
- ♿ `prefers-reduced-motion` ahora también desactiva corazones, GIF, hovers y la barra de scroll
- ⚙️ Relojeo del contador sin deriva y resincronización al recuperar la pestaña
- 🔢 Cifras del contador forzadas a `lining-nums` — Cormorant Garamond renderiza figuras de estilo antiguo con alturas desiguales, lo que hacía saltar visualmente el número de días

### v1.2.0 (2026-09-18)

**Nuevas funcionalidades:**
- 💖 Restaurados corazones flotantes con animación suave
- 🔘 Agregado botón de accesibilidad para activar/desactivar corazones
- ⚙️ Implementado toggle inteligente con estados visuales ON/OFF
- 🧹 Limpieza automática de corazones al desactivar

**Correcciones:**
- 🔧 Ajustado `prefers-reduced-motion` para no ocultar corazones por defecto
- 🗣️ Actualizados textos de interfaz a español neutro

### v1.1.0 (2026-09-18)

**Correcciones:**
- 🐛 Corregido bug crítico que impedía la reproducción del audio
- 🐛 Eliminadas funciones duplicadas en el JavaScript

**Mejoras:**
- ♿ Agregado soporte `prefers-reduced-motion` para accesibilidad
- 🖼️ Agregadas dimensiones a todas las imágenes para prevenir CLS
- 🎨 Mejorado contraste de texto secundario para mejor legibilidad

### v1.0.0 (2026-09-18)

**Versión inicial:**
- 🎨 Diseño romántico con paleta rosa y dorado
- 📸 Galería de tres momentos especiales
- 🎵 Reproductor de música con tema de Prontera
- 💖 Animación de corazones flotantes
- 📱 Diseño responsive para todos los dispositivos
- ♿ Accesibilidad básica (aria-label, roles, navegación por teclado)

## 🔍 Aspectos técnicos destacados

### Optimizaciones de rendimiento

- **IntersectionObserver**: Las animaciones solo se ejecutan cuando las secciones son visibles.
- **`loading="lazy"`**: Las imágenes se cargan diferidamente.
- **Imágenes en WebP**: las fotos de la galería pesan ~34% menos que en JPEG y declaran sus dimensiones reales para evitar layout shift.
- **Fuentes por rango variable**: una sola petición cubre todos los pesos, sin descargar variantes que la página no usa.
- **Safe areas con `env()`**: los controles fijos se calculan con variables que valen 0 en pantallas sin muesca, sin media queries extra.
- **`will-change`**: Propiedad optimizada para elementos animados.
- **`image-rendering: pixelated`**: Optimización para GIFs de baja resolución.

### Accesibilidad (a11y)

- ARIA labels en todos los elementos interactivos
- Roles semánticos (`role="slider"`, `aria-pressed`, `aria-valuenow`)
- Navegación completa por teclado
- Soporte para usuarios con preferencias de movimiento reducido (semilla del interruptor, no destino final)
- Contraste de colores WCAG AA cumplido
- Botón de toggle con estado accesible (`aria-pressed`) y `aria-label` estable
- Contenido legible aunque nunca se dispare la animación de aparición

### Arquitectura del código

- **CSS Custom Properties**: Fácil mantenimiento y theming
- **Separación lógica**: CSS para estilos, HTML para estructura, JS para comportamiento
- **Código autocontenido**: Sin dependencias externas, fácil de mantener
- **Funciones modulares**: Código organizado en funciones reutilizables

## 📝 Licencia

Este proyecto es personal y está hecho con amor. 💖

---

*"Hecho con ♥ para mi persona favorita"*
