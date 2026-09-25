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
| 🔘 **Botón de accesibilidad** | Toggle para activar/desactivar corazones flotantes |
| 📱 **Diseño responsive** | Se adapta a móviles, tabletas y escritorio |
| ♿ **Accesibilidad** | Cumple estándares WCAG con soporte para `prefers-reduced-motion` |
| 🎭 **Animaciones suaves** | Transiciones con IntersectionObserver para aparición progresiva |
| ⏱️ **Contador de tiempo juntos** | Reloj en vivo con días, horas, minutos y segundos desde el 15 de octubre de 2013 |
| 📊 **Barra de progreso de scroll** | Indicador visual en la parte superior que muestra cuánto se ha recorrido la página |
| 💞 **Separadores decorativos** | Líneas con corazón entre secciones para dar ritmo a la lectura |
| 🔍 **Lightbox de fotos** | Vista ampliada de las fotos de la galería con navegación y teclado completo |
| 🔊 **Control de volumen** | Slider de volumen y botón de silenciar que recuerda el último nivel |
| ✨ **Brillo de la carta** | Resplandor que sigue al cursor sobre la carta final |

## 🔧 Mejoras implementadas

### Fase 1 — Interacción y ritmo visual

- **Contador de tiempo juntos**: Cuatro tarjetas en vivo (días, horas, minutos, segundos) que calculan la diferencia real contra `TOGETHER_SINCE`. Se alinea al borde de cada segundo para evitar deriva y resincroniza al volver a la pestaña, ya que los timers se throtelan en segundo plano. Usa `font-variant-numeric: tabular-nums` para que el layout no salte cada segundo.
- **Barra de progreso de scroll**: Fija en la parte superior, se actualiza dentro de `requestAnimationFrame` con listeners `passive` para no bloquear el scroll. Es decorativa (`aria-hidden`) y desaparece al llegar al final.
- **Separadores decorativos**: Líneas con degradado y un corazón central entre las secciones. Comparten la clase `.section` para heredar la misma animación de aparición.
- **`prefers-reduced-motion` ampliado**: Además de las secciones, ahora desactiva corazones flotantes, el GIF de la ovejita, los efectos hover de tarjetas y botones, y la transición de la barra de scroll.

### Fase 2 — Táctil, sonido y detalle

- **Lightbox de fotos**: Cada foto de la galería es ahora un `<button>` real, así que se abre con teclado y lector de pantalla. Al abrir, el foco salta al botón de cerrar, el fondo queda bloqueado y al salir el foco vuelve a la foto que lo abrió. `Escape` cierra, las flechas navegan con wrap-around, `Tab` queda atrapado dentro de los tres botones y un clic en el fondo también cierra. En móvil los botones de navegación bajan al pie de la foto.
- **Control de volumen**: Slider de volumen con relleno `--fill` y botón de silenciar que conmuta entre el último nivel y cero. El volumen se guarda en `lastVolume` solo cuando es distinto de cero, así que silenciar y volver nunca pierde el nivel anterior. `aria-pressed` y el texto accesible cambian con el estado.
- **Brillo de la carta**: Un resplandor radial sigue al cursor sobre la carta final usando las custom properties `--mx`/`--my`. El listener solo se adjunta si el usuario no pidió movimiento reducido: en ese caso el brillo queda en su posición estática.
- **`.gitignore`**: Ahora ignora `.atl/` para que los metadatos de las herramientas de asistencia no entren al repositorio.

### Corrección de errores críticos

- **Bug del reproductor de audio**: Se corrigió un error donde `audio` se utilizaba antes de ser declarado, lo que impedía la reproducción automática al iniciar la experiencia.

### Funcionalidad de corazones flotantes

- **Corazones por defecto**: Los corazones aparecen automáticamente al cargar la página.
- **Botón de accesibilidad**: Ubicado en la esquina inferior derecha, permite activar o desactivar los corazones con un solo clic.
- **Toggle inteligente**: El botón muestra el estado actual ("Corazones: ON" o "Corazones: OFF") y controla la generación de nuevos corazones en tiempo real.
- **Limpieza automática**: Al desactivar, se eliminan todos los corazones existentes de la pantalla.
- **Preservación de preferencias**: Respeta la configuración `prefers-reduced-motion` del sistema operativo, pero mantiene los corazones activos por defecto para la mayoría de usuarios.

### Accesibilidad

- **`prefers-reduced-motion`**: Las animaciones problemáticas se desactivan automáticamente para usuarios con sensibilidad al movimiento o vértigo.
- **Dimensiones en imágenes**: Todas las imágenes declaran `width` y `height` para prevenir layout shift (CLS).
- **Contraste mejorado**: Color de texto secundario mejorado de `#55424d` a `#4a3644` para un ratio de contraste de 7:1.
- **Navegación por teclado**: El reproductor de música y el botón de accesibilidad son completamente navegables con teclado.
- **Estados ARIA**: El botón de corazones utiliza `aria-pressed` para indicar su estado actual a los lectores de pantalla.

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

4. Usar el botón de accesibilidad (esquina inferior derecha) para activar o desactivar los corazones flotantes

## 📁 Estructura del proyecto

```
├── index.html                    # Página principal (HTML + CSS + JS)
├── README.md                     # Documentación del proyecto
├── .gitignore                    # Archivos ignorados por Git
├── prontera-theme.mp3            # Música de fondo (Tema de Prontera)
├── ovejita.gif                   # GIF decorativo para las tarjetas
├── bubu-dudu.gif                 # GIF de la pareja
├── foto1-primer-encuentro.jpg    # Foto del primer encuentro (2013)
├── foto2-momentos-felices.jpg    # Foto de momentos felices (2019)
├── foto3-siempre-juntos.jpeg     # Foto de siempre juntos (2025)
└── odd/tasks/                    # Documento de seguimiento de mejoras
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

## 📋 Changelog

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
- **`will-change`**: Propiedad optimizada para elementos animados.
- **`image-rendering: pixelated`**: Optimización para GIFs de baja resolución.

### Accesibilidad (a11y)

- ARIA labels en todos los elementos interactivos
- Roles semánticos (`role="slider"`, `aria-pressed`, `aria-valuenow`)
- Navegación completa por teclado
- Soporte para usuarios con preferencias de movimiento reducido
- Contraste de colores WCAG AA cumplido
- Botón de toggle con estado accesible para lectores de pantalla

### Arquitectura del código

- **CSS Custom Properties**: Fácil mantenimiento y theming
- **Separación lógica**: CSS para estilos, HTML para estructura, JS para comportamiento
- **Código autocontenido**: Sin dependencias externas, fácil de mantener
- **Funciones modulares**: Código organizado en funciones reutilizables

## 📝 Licencia

Este proyecto es personal y está hecho con amor. 💖

---

*"Hecho con ♥ para mi persona favorita"*
