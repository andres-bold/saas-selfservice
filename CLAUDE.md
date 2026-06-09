# Bold POS — Prototipo Self-Service

Prototipo navegable de un flujo de suscripción SaaS para Bold, empresa colombiana de pagos y punto de venta. El objetivo es simular la experiencia completa de un comercio que descubre el producto, elige un plan y activa su suscripción.

---

## Estructura de archivos

```
saas_selfservice/
├── index.html          ← Landing principal (GitHub Pages entry point)
├── restaurantes.html   ← Landing especializada en Restaurantes y Bares
├── facturacion.html    ← Pantalla "en diseño" para POS Facturación Lite
├── recomendar.html     ← Paso previo: ¿Te ayudo a elegir? o Quiero armar mi POS
├── planes.html         ← Flujo de suscripción pago (3 pasos: Elige → Resumen → Confirmación)
├── prueba-gratis.html  ← Flujo de activación de prueba gratuita (sin tarjeta)
└── onboarding.html     ← Pantalla "Mi cuenta" para usuarios con sesión activa
```

### Navegación entre páginas

```
index.html / restaurantes.html
  ├── Modal "POS Restaurantes y Bares"  → restaurantes.html
  ├── Nav "POS Restaurantes"            → restaurantes.html
  ├── Nav "POS Facturación Lite"        → facturacion.html
  ├── CTAs "Quiero mi POS" / "Empieza gratis" → recomendar.html
  └── Botón fixed verde "Empieza gratis hoy" → prueba-gratis.html

recomendar.html
  ├── "Ayúdame a escoger" (quiz) → pantalla de recomendación → planes.html
  ├── "Quiero armar mi POS" → planes.html
  ├── 3 botones verticales (Bold POS / Restaurantes / Facturación) → planes.html
  └── Botón fixed verde "Empieza gratis hoy" → prueba-gratis.html

planes.html  (3 pasos en página única)
  ├── Paso 1: Elige plan → Paso 2
  ├── Paso 2: Resumen y pago → Paso 3
  ├── Paso 3: Confirmación (sin botón "empieza gratis")
  └── Botón fixed rojo "💬 Necesito ayuda" → modal WhatsApp (visible en pasos 1 y 2)

prueba-gratis.html
  ├── Formulario → modal "email ya existe" → pantalla confirmación
  └── "Ver todos los planes" → planes.html

facturacion.html
  └── "Volver a Bold POS" → index.html

onboarding.html  (flujo Mi cuenta, 4 pasos)
  ├── Paso 1: Mi suscripción (stepper oculto)
  ├── Paso 2–4: stepper visible (1 Mi suscripción · 2 Elegir plan · 3 Resumen · 4 Confirmación)
  └── Botón fixed rojo "💬 Necesito ayuda" → modal WhatsApp
```

---

## Sistema de diseño (Bold)

Proyecto de referencia: `/Users/andres.lizarazo/and_claude/saas-renewals/index.html`

### Tokens de color

| Variable        | Valor     | Uso                              |
|-----------------|-----------|----------------------------------|
| `--red`         | `#E8003D` | Primario, CTAs, activos          |
| `--red-dark`    | `#C0002E` | Hover de botones rojos           |
| `--red-light`   | `#FFE5ED` | Fondos suaves, chips de alerta   |
| `--navy`        | `#12122A` | Títulos, sidebar, fondos hero    |
| `--blue`        | `#121E6C` | Acentos secundarios              |
| `--gray-bg`     | `#F4F5F8` | Fondo de página, secciones alt   |
| `--gray-line`   | `#E4E6EF` | Bordes, divisores                |
| `--gray-text`   | `#7B7E96` | Texto secundario, placeholders   |
| `--text`        | `#1C1C3A` | Texto principal                  |
| `--white`       | `#FFFFFF` | Fondos de tarjetas               |
| `--green`       | `#00C48C` | Estados positivos, éxito, gratis |
| `--orange`      | `#FF9500` | Advertencias, estados intermedios|

### Tipografía
- Fuente: **Inter** (Google Fonts), pesos 400–900
- Títulos principales: `font-weight: 900`, `letter-spacing: -1.5px` a `-2px`
- Texto base: `font-size: 14–16px`

### Componentes recurrentes
- **Botones primarios**: `background: var(--red)`, `border-radius: 50px`, pill shape
- **Botones outline**: `border: 1.5px solid var(--gray-line)`, fondo blanco
- **Chips de estado**: `border-radius: 20px`, con dot de color antes del texto
- **Tarjetas**: `border-radius: 10–16px`, `border: 1px solid var(--gray-line)`
- **Tarjeta plan destacado (featured)**: fondo `var(--white)`, borde `var(--red)`, sombra roja suave — **nunca fondo oscuro/navy**
- **Visual panels** (`.visual-panel`): fondo `var(--white)`, borde `var(--gray-line)`, sombra suave — **nunca fondo navy**
- **Stats band**: fondo `var(--gray-bg)`, borde top/bottom `var(--gray-line)`, números en `var(--navy)` — **nunca fondo navy**
- **Hero sections**: fondo `var(--navy)` en `index.html` (intencional, se mantiene oscuro); fondo `var(--gray-bg)` en `restaurantes.html` (convertido a light)
- **Sección final CTA / footer**: fondo `var(--navy)` en ambas landings — se mantiene oscuro por decisión de diseño
- **Botón fixed verde superior**: `position:fixed; top:60px; left:50%; border-radius: 0 0 50px 50px` — emerge del navbar (solo en `recomendar.html` y `planes.html` pasos 1–2)
- **FAB "Necesito ayuda"**: `position:fixed; bottom:28px; right:28px`, fondo `var(--red)`, abre modal que simula redirección a WhatsApp. Presente en `planes.html` (pasos 1–2) y `onboarding.html`

### Patrón: modal de calendario de agenda
Reutilizado en `planes.html`, `prueba-gratis.html` y `onboarding.html`:
- Overlay centrado con `z-index` alto
- Grid 4 columnas de días hábiles (8 días, sin fines de semana)
- Cada día en dos líneas: nombre completo del día arriba, `DD mmm` abajo
- Grid 3×2 de horarios (9am–11am, 2pm–4pm)
- Botón "Confirmar sesión" deshabilitado hasta elegir fecha y hora
- Al confirmar: panel lateral muestra fecha/hora + opción "Cancelar cita"

---

## index.html — Landing principal

### Secciones (en orden)
1. **Navbar** fijo — logo Bold + 3 nav links + Iniciar sesión (→ `onboarding.html`) + Quiero mi POS
2. **Modal** — se abre automáticamente a los 0.8s. Anuncia "POS Restaurantes y Bares". Sin botón cerrar (X) ni footer. Estructura: eyebrow → título una línea → grid 3×2 con 6 funcionalidades (solo título, sin descripción) → botón "Conocer más" → mockup light de mesas
3. **Botón fijo** "Quiero mi POS" — esquina inferior derecha, animación de pulso
4. **Hero** — fondo navy (intencional, se mantiene oscuro), mockups light de celular + tablet
5. **Logos band** — "50.000 negocios ya usan Bold POS"
6. **Features × 3** — layout 2 columnas alternadas, visual panels en light
7. **Stats band** — fondo `gray-bg` (light)
8. **Cómo funciona** — 4 pasos con stepper horizontal
9. **Precios** — tabs de producto + toggle trimestral/anual; plan Avanzado (featured) en light
10. **Testimonios**, **FAQ**, **CTA final** (navy), **Footer** (navy)

### Sección de precios
- **Tabs**: Bold POS | POS Restaurantes | POS Facturación Lite — JS `switchPlanTab()`
- **Toggle**: Trimestral (default) | Anual (Ahorra 25%) — CSS puro con clase `period-anual`
- **No existe opción mensual** en landings. El periodo base es trimestral.

---

## restaurantes.html — Landing Restaurantes y Bares

Mismo layout que `index.html` con contenido especializado en restaurantes. Botón "Iniciar sesión" apunta a `onboarding.html`.

- **Hero**: fondo `gray-bg` (light) — diferente a `index.html` que es navy. El big-mockup de pantalla del restaurante se mantiene oscuro (#1A1A36) como elemento de UI dentro del hero claro.
- **Visual panels**: light (igual que `index.html`)
- **Stats band**: light (igual que `index.html`)
- **Plan featured**: light, borde rojo (igual que `index.html`)
- **CTA final**: fondo navy (igual que `index.html`)
- **Footer**: idéntico al de `index.html` (navy, mismos links)

---

## facturacion.html — En diseño

Pantalla de espera navy full-screen. Icono animado, barra de progreso (65%, Q3 2026), CTA "Avísame cuando esté lista". Botón "Iniciar sesión" apunta a `onboarding.html`.

---

## recomendar.html — ¿Por dónde empezamos?

Layout dividido 50/50:

### Panel izquierdo (navy) — Quiz guiado
- Q1: texto libre del negocio → Q2–Q5 selección con auto-avance
- Al finalizar: algoritmo recomienda plan → **tarjeta light** (fondo blanco, borde rojo) + CTA → `planes.html`

### Panel derecho (gris) — Self-serve
- 3 botones de vertical → todos a `planes.html`

### Algoritmo de recomendación
```
keywords restaurante → POS Restaurantes (Avanzado o Ilimitado)
inventario=no + facturación≠no → POS Facturación Lite Avanzado
multi-sede o muchos usuarios → Bold POS Ilimitado
inventario=alto → Bold POS Avanzado
default → Bold POS Esencial
```

---

## planes.html — Flujo de suscripción (3 pasos)

Flujo de compra para usuarios nuevos.
- **Botón fixed verde** "Empieza gratis hoy · Sin tarjeta" → `prueba-gratis.html` (oculto en paso 3)
- **FAB rojo** "💬 Necesito ayuda" → modal WhatsApp (visible en pasos 1 y 2)

### Paso 1 — Elige tu plan
- **Tabs de producto**: Bold POS / POS Restaurantes / POS Facturación Lite
- **Descripción de vertical**: icono flotante + nombre + texto + chips
- **3 planes por producto**: elementos fijos + configurables (select) + add-ons (toggle) + precio dinámico
- Botón "Continuar" deshabilitado hasta seleccionar

### Paso 2 — Resumen y pago
- **Columna izquierda**: datos del negocio (nombre, negocio, correo, teléfono, NIT, "¿Qué vendes o qué haces?" — los dos últimos en la misma fila)
- **Método de pago**: Tarjeta (con form) / PSE (con select banco + correo con opción "Completar con datos del negocio")
- **Columna derecha sticky** con 3 secciones:
  1. **Resumen del pedido**: nombre del plan, líneas de base y extras, tabs Trimestral/Anual (−20%), total grande + equivalente mensual en paréntesis, nota de cobro. Sin IVA.
  2. **¿Tienes un cupón?**: input + botón Verificar (código demo: `BOLD20`)
  3. **¿Un experto te apoyó?**: campo de texto para nombre del asesor

### Paso 3 — Confirmación
- Layout 2 columnas: izquierda (éxito + detalles del plan + correo enviado con timer 2:00 + editar correo inline) / derecha (agenda de implementación con experto)
- **Precio label** dinámico: "Precio trimestral" o "Precio anual" según selección
- **Calendario de agenda**: modal central, 8 días hábiles + 6 horarios. Al confirmar: fecha/hora grande + "Cancelar cita". "Recuérdame después" colapsa el panel.

### Pre-selección desde recomendación
- `sessionStorage.boldpos_selected_plan` → activa tab + selecciona plan
- `sessionStorage.boldpos_goto_step = "2"` → salta al paso 2

---

## prueba-gratis.html — Activación gratuita (sin tarjeta)

Flujo de prueba gratuita de 15 días. **Sin validación de formulario** (para facilitar demos — el botón siempre está habilitado).

### Pantalla de formulario
- Banner verde oscuro, selector Restaurante/Comercio, formulario 6 campos
- **Columna derecha sticky** (en orden):
  1. **Agenda sesión demo**: tarjeta con descripción + botón "Agendar sesión demo" → abre modal de calendario. Al confirmar muestra fecha/hora + "Cancelar cita". Sin "Recuérdame después".
  2. **Resumen del pedido**: Total $0, sin IVA, sin tarjeta
- **Botón "Activar"**: 1er click → modal "email ya existe"; 2do click → confirmación
- Si no hay correo escrito, la confirmación usa `correo@cliente.com` como valor simulado

### Pantalla de confirmación
- **Layout 2 columnas** sobre fondo navy:
  - Izquierda: título + subtítulo (ancho completo encima) → email card centrado + reenvío 2:00 + editar correo + link "Ver todos los planes"
  - Derecha: tarjeta de agenda (misma lógica: botón → modal → fecha confirmada con cancelar)

---

## onboarding.html — Flujo Mi Cuenta (usuarios con sesión)

Usuario simulado: **Restaurante Josefina** (POS1235456). Acceso vía "Iniciar sesión" en cualquier navbar.

### Paso 1 — Mi suscripción (stepper oculto)
- **Saludo**: "Hola, Restaurante Josefina 👋" con subtítulo de bienvenida
- **Alerta** de vencimiento (14 días): fecha "18 de junio de 2026" en `white-space:nowrap`
- **Plan actual**: Emprendedor 3000 Silver Pro — precio anual `$1.068.000/año`, sin campo "Vertical"
- **Botón "Renovar o cambiar plan"** → abre **modal de decisión**:
  - "Renovar tal como está" → pre-selecciona `rest-avanzado` y salta al paso 3
  - "Ajustar condiciones" → va al paso 2
- **Sesión de implementación**: tarjeta navy degradado con mensaje "Aún no has tenido tu sesión con un experto, no esperes más" + botón "Agendar ahora" → abre modal de calendario
- **Uso del período**: 3 métricas con barras de progreso (facturas 63%, comprobantes 80%, usuarios 60%)
- **Métodos de pago**: lista de tarjetas (Visa 4242 principal + Mastercard 7890) con acciones:
  - "Hacer principal" / chip "Principal"
  - "Eliminar" (bloqueado en la principal)
  - "+ Agregar tarjeta" → formulario inline

### Paso 2 — Elegir plan (stepper visible)
- **Tabs de vertical**: POS Restaurantes (activo, "Tu vertical actual") / Bold POS / POS Facturación Lite
- **Descripción de vertical**: icono flotante + nombre + texto + chips de color por vertical
- **3 planes configurables** por vertical (misma mecánica que `planes.html`):
  - Elementos fijos, selects configurables, add-on toggles, precio dinámico
  - Plan **Restaurantes Avanzado** marcado con badge verde "Tu plan actual"
- Botón "Continuar" deshabilitado hasta seleccionar; "Cancelar" vuelve al paso 1

### Paso 3 — Resumen y pago (stepper visible)
- Detalle de suscripción + método de pago (tarjeta guardada / nueva tarjeta / PSE)
- **Order summary sticky**: nombre del plan, líneas base/extras, tabs Trimestral/Anual (−20%), total + equivalente mensual en paréntesis, nota de cobro. Sin IVA.

### Paso 4 — Confirmación (stepper visible)
- Éxito con detalles: plan, período, inicio, próxima renovación, total cobrado, método
- Botones: "Ir a mi suscripción" / "Descargar comprobante"

---

## Decisiones de diseño tomadas

- **Tabs de precios en landing**: JS `switchPlanTab()` — no navegan a otras páginas.
- **Toggle trimestral/anual en landings**: CSS puro con clase `period-anual`. Sin opción mensual.
- **Precios en `planes.html`**: en `/mes` porque es el flujo de compra real.
- **Período de facturación en `planes.html` paso 2 y `onboarding.html` paso 3**: tabs JS Trimestral/Anual dentro del order summary. Anual aplica −20%. Sin IVA en ambos flujos.
- **Modal de restaurantes** en `index.html`: se abre a los 0.8s, se cierra al hacer clic en el backdrop. Sin botón X ni footer. Título en una sola línea (`white-space: nowrap`). Las 6 funcionalidades muestran solo el título, sin descripción.
- **`facturacion.html`** es intencionalmente minimalista — pantalla de espera.
- **`recomendar.html`** es el entry point de todos los CTAs principales.
- **Sesión simulada en `prueba-gratis.html`**: modal "email ya existe" en 1er click, procede en 2do. Formulario sin validación para demos.
- **FAB "Necesito ayuda"**: rojo Bold, fijo abajo-derecha, abre modal que simula redirección a WhatsApp. No aparece en landings ni en paso 3 de `planes.html`.
- **Patrón de agenda de sesión**: reutilizado en 3 archivos. Modal central con calendario, panel lateral muestra solo estado (botón o fecha confirmada). Sin "Recuérdame después" en `prueba-gratis.html`.
- **Stepper en `onboarding.html`**: completamente oculto en el paso 1; aparece desde el paso 2 mostrando los 4 pasos con numeración correcta y checks en los completados.
- **Numeración del stepper**: siempre derivada del `id` del step (`step-N`), nunca de `indexOf`, para evitar bug de renumeración.
- El proyecto se despliega en **GitHub Pages** con `index.html` como entry point.

---

## Estado actual

| Página              | Estado       | Notas                                                                 |
|---------------------|-------------|------------------------------------------------------------------------|
| `index.html`        | ✅ Completo  | Landing light · modal restaurantes simplificado · planes featured light |
| `restaurantes.html` | ✅ Completo  | Landing light (hero gray-bg) · big-mockup oscuro · planes featured light |
| `facturacion.html`  | ✅ Completo  | Pantalla "en diseño"                                                    |
| `recomendar.html`   | ✅ Completo  | Split 50/50 · tarjeta recomendación light                              |
| `planes.html`       | ✅ Completo  | 3 pasos · planes featured light · pago sin IVA · agenda · FAB ayuda   |
| `prueba-gratis.html`| ✅ Completo  | 2 pantallas · sidebar con agenda demo · confirmación 2 columnas        |
| `onboarding.html`   | ✅ Completo  | Mi cuenta 4 pasos · planes featured light · modal renovar · agenda     |

**Próximo paso pendiente**: diseñar el contenido completo de `POS Facturación electrónica Lite` para reemplazar la pantalla de espera en `facturacion.html`.
