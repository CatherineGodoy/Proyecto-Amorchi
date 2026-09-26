# Feature: Mejoras premium — Página de aniversario

## Objetivo

Llevar la página romántica del 14° aniversario (Proyecto Amorchi) al siguiente nivel con
mejoras de impacto alto, manteniendo el stack **HTML/CSS/JS puro, sin framework**.

## Problema / Por qué

La usuaria quiere hacer un lindo regalo a su pareja. La página base (v1.2.0) ya funciona y
tiene accesibilidad resuelta, pero carece de los detalles que hacen que se sienta "premium":
un contador vivo del tiempo juntos, feedback visual de scroll, y separadores que le den
ritmo a la lectura.

## Decisiones registradas

| Decisión | Resultado |
| --- | --- |
| ¿Migrar a React? | **No.** Se queda en HTML puro. El peso real del proyecto es el MP3 (8.9 MB), no el framework; React agregaría hidratación y build sin beneficio para una vista estática. |
| Estrategia de entrega | Commits de trabajo directos en `main` (histórico del repo). Estrategia `ask-on-risk`: si el acumulado supera ~400 líneas autorizadas, preguntar antes de continuar. |
| TDD estricto | **Apagado.** No hay `sdd-init`, ni framework de tests, ni `package.json`. Verificación funcional ordinaria. |
| Ruta de implementación | **Inline directo** — el trabajo toca un único archivo no trivial (`index.html`), no dispara el trigger de delegación por volumen de archivos. |

## Alcance

**En alcance:** las 4 fases de mejoras + README + commits de trabajo.
**Fuera de alcance:** migración a framework, backend, dominio propio, cambios en el texto de la carta
(a menos que la usuaria lo pida).

## Restricciones

- Textos de la página en **español neutro**, sin modismos argentinos.
- **SIEMPRE** actualizar `README.md` después de cada cambio.
- Respetar `prefers-reduced-motion` y las mejoras de accesibilidad ya existentes.
- No romper el botón "Comenzar nuestra historia" ni el player de música.
- Commits convencionales, sin atribución a IA.

## Checklist

### Fase 1 — Alta prioridad

- [x] **T1** Contador de tiempo juntos en vivo (días, horas, minutos, segundos) — fecha `2013-10-15`
- [x] **T2** Barra de progreso de scroll en la parte superior
- [x] **T3** Separadores decorativos entre secciones

### Fase 2 — Media prioridad

- [x] **T4** Lightbox: al clickear una foto, abrirla a pantalla completa
- [x] **T5** Control de volumen en el player de música
- [x] **T6** Hover con brillo sutil en la carta

### Fase 3 — Pulido visual

- [x] **T7** Parallax sutil en el fondo
- [x] **T8** Línea de tiempo conectando 2013 → 2019 → 2025
- [x] **T9** Sello decorativo (SVG) en la carta

### Fase 4 — Performance

- [ ] **T10** Optimizar imágenes a WebP (objetivo: −40/60% de peso)
- [ ] **T11** Reducir familias/variantes de fuentes
- [ ] **T12** Safe areas para notch (`env(safe-area-inset-*)`)

### Cierre

- [x] **T13** README actualizado
- [x] **T14** Commit de trabajo por unidad terminada

### Pedidos puntuales

- [x] **T15** Ovejita de la galería: que se mueva sola y además reaccione al clic
- [x] **T16** Interruptor general de animaciones (reemplaza el bloque `prefers-reduced-motion`)

## Criterios de aceptación

1. La página carga sin errores de consola en navegador de escritorio y móvil.
2. El contador avanza en tiempo real y sobrevive a cambiar de pestaña.
3. La barra de scroll refleja el progreso real y desaparece al 100%.
4. Las secciones siguen apareciendo con la animación existente, y con las animaciones apagadas
   (vía sistema o vía botón) se muestran **completas** — nunca queda contenido oculto.
5. Todo el texto visible sigue en español neutro.

## Verificación

No hay framework de tests. Verificación funcional por tarea:

```bash
# 1. Sintaxis del JS inline (extrae <script> y lo valida con Node)
powershell -Command "$h=Get-Content -Raw index.html; $j=[regex]::Match($h,'(?s)<script>(.*?)</script>').Groups[1].Value; Set-Content -Path $env:TEMP\inline.js -Value $j -Encoding UTF8; node --check $env:TEMP\inline.js"

# 2. Estructura básica del HTML
powershell -Command "$h=Get-Content -Raw index.html; foreach($t in 'section','article','button'){ $o=([regex]::Matches($h,\"<$t[ >]\")).Count; $c=([regex]::Matches($h,\"</$t>\")).Count; \"$t: $o abierto / $c cerrado\" }"

# 3. Peso de assets (antes/después de la Fase 4)
powershell -Command "Get-ChildItem *.jpg,*.jpeg,*.gif,*.png -ErrorAction SilentlyContinue | Measure-Object Length -Sum | ForEach-Object { '{0:N1} KB total' -f ($_.Sum/1KB) }"
```

Verificación manual (usuario): abrir `index.html` y validar los criterios 1–5.

## Progreso

| Tarea | Estado | Evidencia |
| --- | --- | --- |
| T1 | ✅ Hecha | `node` sobre la lógica pura: `4727 días \| 20h \| 39m` correcto; borde `t=inicio` → todo en 0; reloj atrasado → clamp a 0 |
| T2 | ✅ Hecha | Listener `passive` + `requestAnimationFrame`; `aria-hidden`; ancho recalculado en `resize` |
| T3 | ✅ Hecha | 3 separadores (`hero→momentos`, `momentos→carta`, `carta→música`), clase `.section` para heredar la animación |
| T4 | ✅ Hecha | 3 fotos pasaron de `<div>` a `<button>`; apertura, `Escape`, flechas con wrap-around, trampa de `Tab`, clic en fondo, bloqueo del scroll y devolución del foco — todo PASS |
| T5 | ✅ Hecha | Slider con `--fill`, botón de silenciar con `lastVolume`, `aria-pressed`/`aria-label`/`title` según estado |
| T6 | ✅ Hecha | `--mx`/`--my` seteados por `pointermove` (69.99% / 20% medido); el listener está siempre adjunto y consulta `animacionesEncendidas()` (antes se condicionaba a `prefers-reduced-motion` al cargar) |
| T7 | ✅ Hecha | Parallax a dos velocidades con `--scroll-p` (160px / 460px), `inset` negativo igual al recorrido para que nunca se descubra el borde |
| T8 | ✅ Hecha | Línea de 2px con degradado + nodos de 14px; nodos alineados al píxel con la línea (250/250 desktop, 39/39 móvil) y sin pisar ninguna tarjeta |
| T9 | ✅ Hecha | Lacre SVG en flujo con `role="img"`; dibujo `5,5 110x110` y texto `34,81 53x15`, ambos dentro del `viewBox`; no tapa firma ni GIF |
| T10–T12 | ⏳ Pendiente | Fase 4 |
| T13 | ✅ Hecha | README: tabla de características, sección "Fase 1", changelog v1.3.0, sección de personalización de fecha, estructura del proyecto |
| T14 | ✅ Hecha | `c29d564` feat · `536be05` fix contador · `5912f4c` + `eb7359c` docs (Fase 1) · `3478998` feat (Fase 2) — los 5 en `origin/main` |
| T15 | ✅ Hecha | Ovejita: `ovejaVuelta` autónoma + `ovejaSalto` al clic, sin colisión con foto/título/tarjeta — 39/39 PASS en 3 anchos |
| T16 | ✅ Hecha | Interruptor general de animaciones: clase `anim-off` en `<html>` reemplaza a `@media (prefers-reduced-motion)`, decisión en `<head>` sin parpadeo, elección en `localStorage` — 35/35 PASS en 2 corridas |

### Verificación ejecutada (Fase 1)

```
JS inline: sintaxis OK          (node --check sobre el <script> extraído)
section:   4/4 OK · article 3/3 OK · button 3/3 OK · div 35/35 OK
braces CSS: 97/97 OK
lógica (node): 4727 días · borde t=inicio → todo en 0 · reloj atrasado → clamp a 0
```

**Verificación visual (headless Edge `--screenshot`):** se capturó el viewport en desktop
(1280px) y móvil (390px). Confirmado: contador en vivo, separadores con corazón arriba y
abajo, secciones reveladas, toggle de corazones operativo.

**Defecto encontrado y corregido en la revisión visual:** Cormorant Garamond renderiza
figuras de estilo antiguo (alturas desiguales), por lo que `4727` mostraba el `2` elevado y
las celdas se veían desparejas. Corregido con `font-variant-numeric: lining-nums tabular-nums`
+ `font-feature-settings:"lnum" 1,"tnum" 1`. Commit `536be05`.

### Verificación ejecutada (Fase 2)

**Estructural:**

```
lineas totales: 1066        (801 al cerrar Fase 1 → +265)
node --check:   OK
etiquetas:      button 10/10 · div 34/34 · section 4/4 · main 1/1 · figure 1/1 · figcaption 1/1
braces CSS:     136/136 OK
ids del JS:     23/23 presentes en el HTML
```

**Funcional (headless Edge `--dump-dom`, 26 aserciones):**

```
26 PASS / 0 FAIL
T4  abre lightbox · foco en cerrar · body bloqueado · caption y src cargados
    ArrowRight cambia · ArrowLeft vuelve · wrap-around · trampa de Tab
    Escape cierra · devuelve foco · desbloquea body · click en fondo cierra
T5  volumen 40 → audio.volume 0.4 · relleno --fill 40%
    silencia a 0 · aria-pressed true · label "Activar sonido"
    restaura a 0.4 · desde 0 restaura lastVolume · volumen 100
T6  --mx y --my seteados (69.99% / 20%) · ::before usa radial-gradient con la posición
```

**Visual (headless Edge `--screenshot`):** reproductor con el control de volumen visible
y lightbox con foto, pie, botones de navegación y cierre. El overlay del lightbox cubre
todo el viewport (medido: RGB constante en el borde izquierdo de arriba a abajo).

**Dos trampas del entorno de verificación que no eran defectos del producto:**

1. Edge headless reporta `prefers-reduced-motion: reduce`, así que el guard de T6 **impide**
   adjuntar el listener por diseño. Para verificarlo hubo que parchar `window.matchMedia` en
   la copia de prueba para que devuelva `matches: false`.
2. El lector de imágenes devolvió mídia cacheada en varias lecturas; la verificación se
   rehizo con `--dump-dom` (texto) y muestreo de píxeles con `System.Drawing`, que no
   envejecen.

### Pedido puntual (T15) — Ovejita de la galería

Pedido de la usuaria el 25/09/2026: en Ragnarok Online la ovejita de clase Genetic se
movía sola en su pantalla y además se podía arrastrar. Quería que la de la página se
moviera **sola**, pero **por donde está** y sin interferir con la foto.

**Decisión:** de las tres opciones ofrecidas se aceptó la 2 — camino cerrado tipo elipse
dentro del propio hueco. Descartadas: una sola ovejita recorriendo toda la galería,
órbita con `<span>` contenedor, y giro en el lugar sin desplazamiento. Después la usuaria
preguntó "¿y si se mueve al hacer clic?" → se implementó **ambas** (autónoma + reacción)
en lugar de reemplazar: solo-click habría dejado el ítem quieto casi siempre y nadie habría
descubierto que era clicable.

**Implementación:**

- `pointer-events: none` → `cursor: pointer` + `user-select: none` (ahora es clicable a propósito).
- `@keyframes ovejaVuelta` (4s, bucle): elipse de 24×7px centrada en `(0, -1.5px)` para que
  trepe más de lo que baja. En escritorio el hueco de la fila da 24px; en móvil la fila pasa a
  columna con `gap .8rem` y `margin-bottom:-5px`, o sea 7.8px de holgura, y el recorrido nunca
  baja de `-5px`, así que la separación mínima medida queda en 4.4px.
- `@keyframes ovejaSalto` (700ms, `linear`): giro completo (`-6deg` → `354deg`) y salto de hasta
  17px hacia arriba. Termina en `translate(0,-5px) rotate(-6deg) scale(1)`, que es exactamente el
  `0%` de `ovejaVuelta`, así que el retome es invisible.
- Arriba hay **40px libres** en las tres filas: `margin-bottom: 2.5rem` del título en la fila 1 y
  `gap: 2.5rem` entre filas en las otras dos — por eso el salto solo trepa y nunca baja.
- `.oveja-gira` toma el `animation-name` durante 700ms y lo suelta con `animationend`. Se suma un
  `setTimeout` de 750ms como respaldo: si `animationend` no llega (pestaña en background, o
  `prefers-reduced-motion` activado a mitad de vuelta) la clase quedaría puesta y la ovejita se
  clavaría quieta para siempre.
- Con las animaciones apagadas el CSS pone `cursor: default` y el listener consulta
  `animacionesEncendidas()` al vuelo, para no prometer un clic que no haría nada.

**Verificación (T15):**

**Estructural:** `lineas: 1125` · `node --check: OK` · etiquetas balanceadas ·
`braces CSS: 150/150` · keyframes `ovejaVuelta` + `ovejaSalto` · `floatOveja: 0` residuales.

**Funcional (headless Edge `--dump-dom`, 39 aserciones en 3 anchos):**

```
39 PASS / 0 FAIL        escritorio 1280 · tablet 768 · celular 390
A  animation-name=ovejaVuelta · pointer-events=auto · cursor=pointer · 3 ovejitas
   se mueve sola · recorrido X 23/23/23px · recorrido Y 7/7/7px
B  clase aplicada → animationName=ovejaSalto · duration=0.7s
   termina sola (clase retirada) → vuelve a animationName=ovejaVuelta
   NUNCA choca con foto/título/tarjeta (rectángulos sin intersección en todas las muestras)
```

**Tres errores del propio test, no del producto:** (a) medí el recorrido juntando los centros
de las tres ovejitas en un solo min/max — están en filas distintas, por eso daba 760px y 1196px,
y el rango se midió después por ovejita; (b) mi override del test forzaba la animación con
`!important` y pisaba `.oveja-gira`, dejando el `animationName` en `ovejaVuelta`; (c) `NodeList`
no tiene `.concat`. Los tres se corrigieron quitando el bloque
`@media (prefers-reduced-motion: reduce)` de la copia de prueba, con lo que la verificación corre
por la misma ruta de CSS que ve un usuario normal.

### Pedido puntual (T16) — Interruptor general de animaciones

**El problema, en dos partes:**

1. **Un bug real.** El botón decía `Corazones: ON` mientras `prefers-reduced-motion` escondía
   todo con `display: none !important` — o sea, un control que miente sobre su propio estado.
2. **Un hallazgo del entorno.** La usuaria reportó que la ovejita no se movía. Verificado: su
   Windows 11 (build 26200) tiene **Configuración → Accesibilidad → Efectos visuales →
   Efectos de animaciones** desactivado, lo que hace que `matchMedia('(prefers-reduced-motion:
   reduce)')` devuelva `true` y por lo tanto se active el bloque completo de reposo.

**Decisión (aceptada por la usuaria):** convertir el toggle de corazones en un **interruptor
general de animaciones** que arranca según el sistema pero que el usuario puede pisar, con la
elección persistida. Ventaja sobre solo documentar el path de Windows: el regalo se controla
desde la propia página, sin depender de una configuración del SO.

**Implementación:**

- **Una sola fuente de verdad:** la clase `html.anim-off`. El bloque
  `@media (prefers-reduced-motion: reduce)` (52 líneas) se convirtió en reglas
  `html.anim-off ...` con `!important` — 0 referencias residuales del mecanismo viejo.
- **Sin parpadeo:** un script de 11 líneas en `<head>` decide y aplica la clase antes del
  primer pintado. Prioridad: `localStorage('amorchi-animaciones')` → `prefers-reduced-motion`
  → encendido.
- **Red de seguridad:** `html.anim-off .section { opacity: 1 !important }` — el contenido
  nunca queda oculto aunque el `IntersectionObserver` no dispare.
- **Botón honesto:** `id="toggleAnimations"`, `aria-label="Animaciones"` estable y
  `aria-pressed` llevando el estado; el texto visible es `Animaciones: ON/OFF`. Así el
  `title`/`aria-label` no cambian en cada toggling (menos ruido en lectores de pantalla) y
  el estilo `.accessibility-toggle[aria-pressed="false"]{opacity:.7}` sigue funcionando.
- **Reacción en vivo:** `spawnHeart`, `startHearts`, el brillo de la carta y el clic de la
  ovejita consultan `animacionesEncendidas()` al momento de actuar, así que el interruptor
  surte efecto sin recargar. `heartsEnabled` (flag duplicado) se eliminó.
- **`localStorage` en `file://`:** verificado con un sondeo (`VAL=1` → `VAL=2` con el mismo
  `--user-data-dir`), así que la elección se recuerda aunque se abra el archivo directo.

**Verificación (T16):**

**Estructural:** `lineas: 1157` · `node --check: OK` en los **2** bloques de `<script>` (11 y
401 líneas) · llaves CSS `149/149` · etiquetas balanceadas (`<html>` 2/2, `<head>` 2/2 — los
contadores dan 2 y 4 porque hay coincidencias dentro de comentarios) · residuos del refactor
`@media (prefers-reduced-motion` / `toggleHearts` / `heartsEnabled` / `motionQuery` /
`Corazones: ON`: **0** · huellas nuevas `animacionesEncendidas` ×7, `anim-off` ×23,
`toggleAnimations` ×9, `amorchi-animaciones` ×2.

**Funcional (headless Edge `--dump-dom`, 35 aserciones, 2 corridas con perfil compartido):**

```
corrida 1 (localStorage vacío)          30 PASS / 0 FAIL
  arranca APAGADAS (default del sistema) · botón "Animaciones: OFF" · aria-pressed=false
  ovejita animation-name=none · cursor=default · 0 corazones
  8/8 secciones en opacity 1 SIN .visible      <-- la garantía crítica
  lightbox abre y cierra con todo apagado
  prender → quita anim-off · "ON" · aria-pressed=true · localStorage=on
  ovejita animation-name=ovejaVuelta · cursor=pointer · playState=running
  recorrido medido 24×7px · nacen corazones · sección 0 visible
  clic → ovejaSalto → vuelve a ovejaVuelta
  apagar → anim-off · "OFF" · localStorage=off · ovejita=none · corazones limpiados
  8/8 secciones siguen visibles con todo apagado · deja ON para la corrida 2

corrida 2 (mismo perfil)                 5 PASS / 0 FAIL
  guardado=on · NO arranca en anim-off aunque el sistema pida menos
  "Animaciones: ON" · ovejita=ovejaVuelta · cursor=pointer · nacen corazones

TOTAL: 35 PASS / 0 FAIL
```

**Límite del entorno de verificación (no es un defecto del producto):** en `--dump-dom` sin
frames, toda animación creada **después** del load queda con `startTime = null` y no avanza
sola — se detectó porque tanto la ovejita como los corazones (código viejo, no tocado)
reportaban `curr=0` mientras `document.timeline.currentTime` sí corría. Por eso el recorrido
se mide **búsqueda de tiempo** (`an.currentTime = t` en 8 puntos y `getBoundingClientRect`)
en lugar de esperar que el tiempo corra: prueba las keyframes exactamente, sin depender de
que el harness produzca frames. `playState: running` confirma que el navegador la va a correr.

### Verificación ejecutada (Fase 3 — T7, T8, T9)

**Estructural:** `lineas: 1218` (1157 al cerrar T16 → **+61**) · `node --check: OK` en los 2
bloques · llaves CSS `155/155` · etiquetas balanceadas (`section 4/4`, `article 3/3`,
`div 34/34`, `svg 9/9`; el contador de `button` da 11/10 solo por un `<button>` escrito
dentro de un comentario CSS, los reales son 10/10).

**Funcional (headless Edge `--dump-dom`, 40 aserciones × 3 anchos = 120):**

```
40 PASS / 0 FAIL    escritorio 1280 · tablet 768 · celular 390

T7  fondo con inset -460px y 1681px de alto (holgura para desplazarse)
    scroll a mitad -> --scroll-p = 0.5001
    capa de corazones = -230.046px   (= -460 x 0.5, exacto)
    capa de gradiente =  -80.016px   (= -160 x 0.5, exacto)
    DOS velocidades distintas -> parallax real
    el fondo CUBRE el viewport (top=-690 bottom=991 vs vh=761)
    con animaciones apagadas: transform=none en ambas y sigue cubriendo

T8  la linea existe, mide 2px y lleva degradado
    nodo 14x14 con radio 50% y borde rgb(214,51,132)
    las 3 filas tienen nodo y los 3 caen SOBRE la linea
      (nodo X = linea X: 250/250 desktop, 39/39 movil)
    nodos DENTRO del contenedor (243.0 >= 227.0 desktop, 32.0 >= 24.0 movil)
    la linea no pisa ninguna tarjeta (252 <= 273 desktop)
    la linea recorre casi toda la columna (1732px, top=6 bottom=6)

T9  sello con viewBox 0 0 120 120, role=img y aria-label
    2 circulos concéntricos, corazon relleno rgb(214,51,132)
    texto "14 AÑOS"
    el dibujo cabe en el viewBox (5,5 110x110)
    el texto cabe y se lee dentro del sello (34,81 53x15)
    NO tapa la firma, NO tapa el GIF, queda dentro de la carta

REG contador con valor · barra de scroll al 50% · lightbox abre y cierra
    ovejita animando y clicable · corazones nacen · boton dice ON
    8/8 secciones visibles con todo apagado
```

**Límite del entorno de verificación (segundo hallazgo del mismo tipo):** este harness
**tampoco ejecuta `requestAnimationFrame`**. Se detectó porque T7 daba `p=0` *y* la barra
de scroll (código viejo, intacto) marcaba `0%` — los dos fallan por la misma causa. El test
llama a `updateScrollProgress()` a mano después del `scrollTo`, lo que verifica la lógica
de cálculo y su efecto en el CSS; el pipeline de rAF es el mismo que la barra de scroll
que la usuaria ya vio funcionando en pantalla.

**Decisión de diseño registrada:** se evaluó `animation-timeline: scroll(root)` para hacer
el parallax **sin JS** (soporte confirmado con `@supports`, pero `transform` no se aplicaba
de forma consistente). Se descartó y se fue por el `requestAnimationFrame` que la página ya
usaba — patrón probado en el repo, cero tecnología nueva.

### ✅ Discrepancia resuelta (ya no bloquea nada)

La usuaria confirmó el origen de las dos cifras:

- **"14 años"** = desde que empezaron a conversar, ~**octubre de 2012** (antes de fin de año).
- **15/10/2013** = **fecha oficial**, la que se eligió conservar para el contador.

Verificado con cálculo: el **15/10/2026** se cumplen exactamente **14 años** desde la
conversación (y 13 desde la fecha oficial). Faltan 21 días desde el 24/09/2026. El rótulo
"14 años" es correcto y **no se modifica ningún texto**.

**Siguiente paso:** Fase 4 (T10 imágenes a WebP, T11 menos familias de fuentes, T12 safe
areas para notch).

**Líneas autorizadas:** Fase 1 cerró en 631 → 801 (**+170**). Fase 2 cerró en 801 → 1066
(**+265**). T15 cerró en 1066 → 1125 (**+59**). T16 cerró en 1125 → 1157 (**+32** — refactor
nulo: se cambió el mecanismo, no se agregó lógica nueva). Fase 3 cerró en 1157 → 1218
(**+61**). Acumulado de la feature: **+587**
sobre las ~400 que eran la referencia de planificación. No se parte la entrega: lightbox,
volumen, brillo, ovejita, interruptor y pulido visual son comportamientos coherentes y la
corrección natural los incluye; además la entrega es commit directo en `main` sin PR, así
que no hay puerta de tamaño que salte. Se registra el sobrepaso para que la fase 4 se
planifique con margen — y conviene que lo sea: T10 (convertir imágenes a WebP) es la que
tiene más potencial de **bajar** líneas y peso.

**Infra:** `.gitignore` ahora excluye `.atl/` (metadatos de herramientas), para que en el
repo solo entre el código de la usuaria.
