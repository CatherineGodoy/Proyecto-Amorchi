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

- [ ] **T4** Lightbox: al clickear una foto, abrirla a pantalla completa
- [ ] **T5** Control de volumen en el player de música
- [ ] **T6** Hover con brillo sutil en la carta

### Fase 3 — Pulido visual

- [ ] **T7** Parallax sutil en el fondo
- [ ] **T8** Línea de tiempo conectando 2013 → 2019 → 2025
- [ ] **T9** Sello decorativo (SVG) en la carta

### Fase 4 — Performance

- [ ] **T10** Optimizar imágenes a WebP (objetivo: −40/60% de peso)
- [ ] **T11** Reducir familias/variantes de fuentes
- [ ] **T12** Safe areas para notch (`env(safe-area-inset-*)`)

### Cierre

- [ ] **T13** README actualizado
- [ ] **T14** Commit de trabajo por unidad terminada

## Criterios de aceptación

1. La página carga sin errores de consola en navegador de escritorio y móvil.
2. El contador avanza en tiempo real y sobrevive a cambiar de pestaña.
3. La barra de scroll refleja el progreso real y desaparece al 100%.
4. Las secciones siguen apareciendo con la animación existente y se muestran completas con `prefers-reduced-motion`.
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
| T4–T12 | ⏳ Pendiente | Fases 2-4 |
| T13 | ✅ Hecha | README: tabla de características, sección "Fase 1", changelog v1.3.0, sección de personalización de fecha, estructura del proyecto |
| T14 | ⏳ Pendiente | Commit de la Fase 1 |

### Verificación ejecutada (Fase 1)

```
JS inline: sintaxis OK          (node --check sobre el <script> extraído)
section:   4 abierto / 4 cerrado OK
article:   3 abierto / 3 cerrado OK
button:    3 abierto / 3 cerrado OK
div:      35 abierto / 35 cerrado OK
style:     1/1   script: 1/1
líneas:    797 (antes: 631)
```

**Pendiente de verificación manual (usuario):** abrir `index.html` y confirmar que el
contador avanza, la barra llena al scrollear y los separadores aparecen con la animación.

### ⚠️ Discrepancia detectada — requiere decisión

La usuaria informó **15/10/2013** como fecha especial. Hoy (2026-09-24) eso son
**12 años cumplidos** (el 13° se cumple el 15/10/2026, en 21 días), pero la página dice
**"14 años juntos"** en el badge, el título, el footer y el README.

Hipótesis: los 14 años se cuentan desde que se conocieron en Ragnarok Online (~2012), y el
15/10/2013 sería el encuentro en persona. **Sin confirmar.** No se modifica ningún texto
de "14 años" hasta que la usuaria lo resuelva.

**Siguiente paso:** confirmar el origen de los "14 años", hacer commit de la Fase 1,
evaluar el límite de 400 líneas autorizadas antes de arrancar la Fase 2.

**Líneas autorizadas estimadas:** ~450–700 (suma de las 4 fases). Fase 1 cerró en **+166
líneas autorizadas** (631 → 797 en `index.html`), dentro del presupuesto.
