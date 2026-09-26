# 💕 Proyecto Amorchi

Página web para celebrar 14 años de amor: una historia contada en tarjetas, con fotos, música de fondo y un contador que corre en vivo desde el 15 de octubre de 2013. Un solo archivo, sin dependencias ni build: se abre y funciona.

## 🚀 Cómo abrirla

```bash
git clone https://github.com/CatherineGodoy/Proyecto-Amorchi.git
```

1. Abrir `index.html` en el navegador.
2. Hacer clic en **Comenzar nuestra historia**.
3. Usar el botón de la esquina inferior derecha (`Animaciones: ON/OFF`) para encender o apagar todo lo animado — la elección queda guardada.

> Si la página arranca sin animaciones es porque Windows tiene "Efectos de animaciones" apagados (**Configuración → Accesibilidad → Efectos visuales**). No hace falta cambiarlo: el botón la enciende por su cuenta.

## 📖 La historia

> *"De la fuente de Prontera al para siempre — cada nivel, cada quest, cada día a tu lado"*

Una historia de amor que comenzó en Ragnarok Online y se convirtió en una aventura para toda la vida.

## ✨ Qué hace la página

| Característica | Descripción |
|----------------|-------------|
| 🎨 **Diseño romántico** | Paleta de colores rosa y dorado con gradientes suaves |
| ⏱️ **Contador de tiempo juntos** | Días, horas, minutos y segundos en vivo desde el 15/10/2013, con cifras tabulares para que el layout no salte cada segundo |
| 📅 **Línea de tiempo** | Línea con nodos que conecta los momentos de 2013, 2019 y 2025 |
| 📸 **Galería de momentos** | Tarjetas con fotos y relatos; la ovejita de cada fila deambula sola y reacciona al clic |
| 🔍 **Lightbox de fotos** | Vista ampliada con navegación por teclado, trampa de foco y devolución del foco al cerrar |
| 🎵 **Reproductor de música** | Tema de Prontera con slider de volumen y botón de silenciar que recuerda el último nivel |
| 💞 **Carta final** | Resplandor radial que sigue al cursor y lacre en SVG que la remata |
| 💖 **Corazones flotantes** | Emojis que ascienden por la pantalla con control de accesibilidad |
| 🏔️ **Fondo con parallax** | Las dos capas del fondo se desplazan a distinta velocidad al hacer scroll |
| 💞 **Separadores decorativos** | Líneas con corazón entre secciones para dar ritmo a la lectura |
| 🔘 **Interruptor de animaciones** | Un solo botón ON/OFF que gobierna toda la página y recuerda la elección |
| 📊 **Barra de progreso de scroll** | Indicador en la parte superior de cuánto se ha recorrido la página |
| 📱 **Diseño responsive** | Se adapta a móvil, tableta y escritorio |
| ♿ **Accesibilidad** | Teclado completo, ARIA en los controles, contraste 7:1 y respeto a *movimiento reducido* |

## 🎨 Cómo personalizarla

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

Para ajustar la velocidad o frecuencia:

```javascript
const FLOAT_ITEM_LIFETIME = 25000;  // Tiempo de vida en milisegundos
const FLOAT_ITEM_INTERVAL = 1200;   // Intervalo entre corazones en milisegundos
```

### Agregar más momentos

Duplica un bloque `.memory-row` en el HTML y modifica la imagen (`src` y `alt`), el título (`h3`), la descripción (`p`) y el año (`memory-footer-year`).

No inviertas el orden de los hijos: la ovejita va **primero** y la tarjeta después, en las tres filas. El patrón alternado (`row-reverse`) se eliminó justamente porque descolocaba la ovejita y desplazaba esa tarjeta respecto a las demás.

## 📁 Estructura del proyecto

```
├── index.html                      # Página principal (HTML + CSS + JS)
├── README.md                       # Documentación del proyecto
├── .gitignore                      # Archivos ignorados por Git
├── prontera-theme.mp3              # Música de fondo (Tema de Prontera)
├── bubu-dudu.gif                   # GIF de la pareja
├── ovejita.gif                     # GIF decorativo para las tarjetas
├── foto1-primer-encuentro.webp     # Foto del primer encuentro (2013, WebP)
├── foto2-momentos-felices.webp     # Foto de momentos felices (2019, WebP)
└── foto3-siempre-juntos.webp       # Foto de siempre juntos (2025, WebP)
```

## 🔧 Detalles técnicos

### Rendimiento

- **Fotos en WebP** (calidad 82): 216 KB → 142 KB (**−34%**). La carga inicial sin música bajó de 355 KB a 281 KB (**−21%**).
- **Sin salto de layout**: cada foto declara su tamaño real, así que el navegador reserva la caja antes de decodificar.
- **Fuentes por rango variable**: una sola petición a Google Fonts cubre todos los pesos que usa la página.
- **Carga diferida**: `IntersectionObserver` anima solo lo que se ve, `loading="lazy"` en las fotos y `will-change` en lo que se mueve.
- **Música fuera del primer render**: el MP3 de 8.9 MB usa `preload="metadata"` y entra recién cuando se reproduce.
- **Safe areas**: los controles fijos se apartan de la muesca y de la barra de inicio con `env()`, y en pantallas sin muesca valen `0px`.
- **GIFs intactos**: convertirlos a WebP con `canvas` dejaría un solo fotograma y mataría la animación.

### Accesibilidad

- **Teclado y lector de pantalla**: las fotos y los controles son `<button>` reales, con `aria-label`, `aria-pressed` y `aria-valuenow` donde corresponde.
- **`prefers-reduced-motion` es la semilla del interruptor**: si el sistema pide menos movimiento, la página arranca apagada. El usuario puede encenderla igual desde el botón, y su elección le gana al sistema.
- **El contenido nunca queda oculto**: con las animaciones apagadas, las secciones quedan en `opacity: 1` aunque el observer nunca dispare — nadie se queda sin poder leer la página.
- **Contraste**: el texto secundario está en ratio 7:1, por encima de WCAG AA.

### Decisiones no obvias

| Decisión | Por qué |
|----------|---------|
| Parallax con `requestAnimationFrame` en vez de `animation-timeline: scroll()` | La propiedad CSS no se comportó de forma consistente en el navegador de verificación. El rAF ya existía para la barra de scroll, así que no se agregó ni un listener ni un loop nuevos. |
| Nodo de la línea de tiempo en `left: -6px`, no `-7px` | Su centro (`left + 7`) debe caer sobre el centro de la línea (`gutter/2 + 1`, porque la línea mide 2px). Con `-7px` quedaba 1px desviado. |
| Sin patrón alternado en las filas de la galería | El `row-reverse` de la fila 2019 ponía su ovejita al lado equivocado y desplazaba esa tarjeta 89px respecto a las demás. |
| Fuentes por rango en lugar de pesos sueltos | Google Fonts ya sirve una única fuente variable: los bytes son los mismos, pero bajan los bloques `@font-face` y el peso `600` deja de quedar clampado a `700`. |
| Una sola clase `anim-off` en `<html>` | Reemplaza al bloque `@media (prefers-reduced-motion)`: una sola fuente de verdad, aplicada sin parpadeo antes del primer pintado y persistida en `localStorage`. |
| Fallback con `setTimeout` en la ovejita | Si `animationend` no llega (pestaña en background), la vuelta se suelta igual para que no quede clavada. |

### Arquitectura

- **Un solo archivo**: HTML, CSS y JS conviven en `index.html`. Sin build, sin dependencias, sin framework.
- **CSS Custom Properties** para colores, tipografías y medidas, con theming centralizado en `:root`.
- **Separación lógica**: CSS para estilos, HTML para estructura, JS para comportamiento.
- **Español neutro** en todos los textos de interfaz.

## 📋 Historial de versiones

| Versión | Fecha | Qué cambió |
|---------|-------|------------|
| 1.6.0 | 2026-09-26 | Fotos a WebP, fuentes por rango variable y *safe areas* para notch |
| 1.5.1 | 2026-09-25 | Las tres ovejitas quedan en el mismo lado; el nodo se centra al píxel |
| 1.5.0 | 2026-09-25 | Parallax a dos velocidades, línea de tiempo y sello en la carta |
| 1.4.2 | 2026-09-25 | Interruptor general de animaciones con persistencia en `localStorage` |
| 1.4.1 | 2026-09-25 | Ovejita que deambula sola y reacciona al clic |
| 1.4.0 | 2026-09-25 | Lightbox con teclado, control de volumen y brillo en la carta |
| 1.3.0 | 2026-09-24 | Contador en vivo, barra de progreso y separadores decorativos |
| 1.2.0 | 2026-09-18 | Corazones flotantes con interruptor y limpieza automática |
| 1.1.0 | 2026-09-18 | Corrección del audio, `prefers-reduced-motion`, dimensiones y contraste |
| 1.0.0 | 2026-09-18 | Versión inicial |

El detalle completo de cada versión está en el historial de commits.

## 🛠️ Tecnologías

| Tecnología | Uso |
|------------|-----|
| HTML5 | Estructura semántica con `section`, `article` y `aria-label` |
| CSS3 | Custom Properties, Flexbox, `clamp()` y `@keyframes` |
| JavaScript vanilla | Sin dependencias externas, 100% nativo |
| Google Fonts | Cormorant Garamond, Great Vibes y DM Sans |

## 📝 Licencia

Este proyecto es personal y está hecho con amor. 💖

---

*"Hecho con ♥ para mi persona favorita"*
