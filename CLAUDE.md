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
  ├── Paso 3: Confirmación → onboarding.html
  └── Botón fixed verde "Empieza gratis hoy" → prueba-gratis.html

prueba-gratis.html
  ├── Formulario → modal "email ya existe" → pantalla confirmación
  └── "Ver todos los planes" → planes.html

facturacion.html
  └── "Volver a Bold POS" → index.html

onboarding.html  (flujo Mi cuenta, 4 pasos)
  └── (flujo interno, sin salida a otras páginas del prototipo)
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
- **Navegación lateral** (backoffice): `border-left: 3px` para ítem activo en rojo
- **Botón fixed verde superior**: `position:fixed; top:60px; left:50%; border-radius: 0 0 50px 50px` — emerge del navbar

---

## index.html — Landing principal

### Secciones (en orden)
1. **Navbar** fijo — logo Bold + 3 nav links + Iniciar sesión + Quiero mi POS
2. **Modal** — se abre automáticamente a los 0.8s. Anuncia "POS Restaurantes y Bares", lista 6 funcionalidades con mockup de plano de mesas. Botones: Cerrar / Conocer más → `restaurantes.html`
3. **Botón fijo** "Quiero mi POS" — esquina inferior derecha, animación de pulso, siempre visible
4. **Hero** — fondo navy oscuro, mockups CSS de celular y tablet mostrando Bold POS en uso
5. **Logos band** — "50.000 negocios ya usan Bold POS" + marcas conocidas
6. **Features × 3** — layout de 2 columnas alternadas (texto + panel visual dark): Facturación electrónica, Inventario en tiempo real, Reportes y cierres
7. **Stats band** — fondo navy: 50k+ negocios, 99% uptime, 24h backups, 4.8★
8. **Cómo funciona** — 4 pasos con stepper horizontal
9. **Precios** — tabs de producto + toggle trimestral/anual (ver detalle abajo)
10. **Testimonios** — 3 tarjetas
11. **FAQ** — acordeón centrado
12. **CTA final** — sección roja con botón blanco
13. **Footer** — 4 columnas sobre fondo navy

### Sección de precios
- **Tabs de producto**: Bold POS | POS Restaurantes | POS Facturación Lite — cambian planes **sin navegar** (JS in-page con `switchPlanTab`)
- **Toggle de periodo**: Trimestral (default) | Anual (Ahorra 25%) — alterna precios con CSS (clase `period-anual` en `#precios`)
- **Planes por producto**:
  - *Bold POS*: Esencial $289k/trim · $963k/año — Avanzado $429k/trim · $1.287M/año — Ilimitado $607k/trim · $1.821M/año
  - *POS Restaurantes*: Esencial $319k/trim · $957k/año — Avanzado $469k/trim · $1.407M/año — Ilimitado $657k/trim · $1.971M/año
  - *POS Facturación Lite*: Básico $149k/trim · $447k/año — Avanzado $229k/trim · $687k/año — Ilimitado $319k/trim · $957k/año
- No existe opción mensual. El periodo base es trimestral.

---

## restaurantes.html — Landing Restaurantes y Bares

### Secciones
1. **Navbar** — mismo que index.html, con "POS Restaurantes" como ítem activo
2. **Botón fijo** "Quiero mi POS"
3. **Hero** — fondo navy, mockup grande de la aplicación mostrando plano de 12 mesas + KDS + strip de stats del turno
4. **Features × 3**: Control de mesas, Pantalla de cocina KDS, Comandas y cobro
5. **Stats band** — igual que index.html
6. **Cómo funciona** — 4 pasos adaptados a restaurantes
7. **Precios** — mismos tabs y toggle que index.html, con panel "POS Restaurantes" activo por defecto
8. **Testimonios** — 3 testimonios de restaurantes y bares
9. **FAQ** — preguntas específicas de restaurantes
10. **CTA final** y **Footer**

---

## facturacion.html — En diseño

Página de espera para "POS Facturación electrónica Lite".
- Fondo navy full-screen, icono animado (float) 🧾
- Barra de progreso pulsante (65%, estimado Q3 2026)
- Chips con funcionalidades previstas
- CTA "Avísame cuando esté lista" + botón volver

---

## recomendar.html — ¿Por dónde empezamos?

Pantalla de entrada al flujo de compra. Layout dividido 50/50:

### Panel izquierdo (navy) — Quiz guiado
- Título: "¿Te ayudo a seleccionar la mejor opción para ti?"
- **Q1**: Texto libre "¿De qué es tu negocio?" → botón Continuar (requiere ≥3 chars)
- **Q2–Q5**: Preguntas de selección con auto-avance al hacer click:
  - Q2: ¿Manejas inventario? (alto / medio / no)
  - Q3: ¿Quieres facturar electrónicamente? (alto / medio / no)
  - Q4: ¿Cuántas sucursales tienes? (1 / 2 / 3 / Más de 3)
  - Q5: ¿Cuántas personas usarán el POS? (1 / 2 / 3 / Más de 3)
- Barra de progreso (5 dots) visible desde Q2. "Omitir → Ver todos los planes" en cada paso.
- Al finalizar → algoritmo de recomendación → pantalla dedicada con el plan sugerido

### Panel derecho (gris) — Self-serve
- Título: "Quiero personalizar mi POS"
- 3 botones de vertical: Bold POS / POS Restaurantes / POS Facturación Lite → todos a `planes.html`

### Algoritmo de recomendación
```
keywords restaurante en texto → POS Restaurantes (Avanzado o Ilimitado)
inventario=no + facturación≠no → POS Facturación Lite Avanzado
multi-sede o muchos usuarios → Bold POS Ilimitado
inventario=alto → Bold POS Avanzado
default → Bold POS Esencial
```

### Pantalla de recomendación (dedicada)
- Plan recomendado con tarjeta navy prominente
- CTA "Continuar con este plan" → guarda `planId` en `sessionStorage` y va a `planes.html` (auto-selecciona y salta al paso 2)
- CTA "Ver todos los planes y personalizar" → `planes.html`

---

## planes.html — Flujo de suscripción (3 pasos)

Flujo de compra para usuarios nuevos.
**Botón fixed verde**: "Empieza gratis hoy · Sin tarjeta" → `prueba-gratis.html`

### Paso 1 — Elige tu plan
- **Tabs de producto**: Bold POS / POS Restaurantes / POS Facturación Lite
- **Descripción de vertical** (entre tabs y planes): icono grande 3D a la izquierda + nombre + texto + chips de capacidades
- **3 planes por producto** con:
  - `plan-sucursales`: "1 Sucursal" / "2 Sucursales" / "3 Sucursales" (entre nombre y descripción)
  - **Elementos fijos** (gris, no interactivos): incluidos en el plan
  - **Elementos configurables** (fondo azul índigo + select): Usuarios, Comprobantes, Facturas — actualizan precio en tiempo real
  - **Add-ons** (fondo verde + toggle): Reportes avanzados, KDS, Datáfono, etc. — actualizan precio
  - Precio dinámico visible en el card. Sin texto "Precio base" (solo aparece nota cuando hay extras)
- Botón "Continuar" deshabilitado hasta seleccionar un plan

### Paso 2 — Resumen y pago
- Layout 2 columnas: formulario izq + order summary sticky der
- Datos del negocio + 2 métodos de pago (tarjeta / PSE)
- Order summary: plan base + opciones extra + IVA 19% + total

### Paso 3 — Confirmación
- Pantalla de éxito → botón "Ir a mi cuenta" → `onboarding.html`

### Pre-selección desde recomendación
En `window.addEventListener('load')`:
- Lee `sessionStorage.boldpos_selected_plan` → activa el tab correcto + selecciona el plan
- Lee `sessionStorage.boldpos_goto_step` = "2" → salta directamente al paso 2

---

## prueba-gratis.html — Activación gratuita (sin tarjeta)

Flujo de prueba gratuita de 15 días.
**Acceso**: botón fixed verde en `recomendar.html` y `planes.html`

### Pantalla de formulario
- **Banner verde oscuro**: "Prueba gratis por 15 días el POS que acelera tu crecimiento"
- **Selector de negocio**: Restaurante 🍽️ / Comercio 🏪 (sin opción Facturación)
- **Formulario**: nombre completo, nombre del negocio, correo, teléfono, NIT/cédula, tipo de negocio (texto)
- **Botones de acción**: Volver + Ver todos los planes + Activar prueba gratuita (deshabilitado hasta llenar todos los campos)
- **Order summary** (sticky, `align-self: start`): "Prueba gratuita Bold POS", precio tachado $89k, descuento −$89k, **Total $0**, sin IVA, solo "🔒 Sin tarjeta requerida"

### Comportamiento del botón "Activar"
- **1er click**: muestra modal "El correo electrónico ya tiene una cuenta de Bold POS" con opciones "Iniciar sesión" → `onboarding.html` o "Continuar de todas formas" → cierra modal
- **2do click** (después de cerrar modal): navega a pantalla de confirmación

### Pantalla de confirmación
- Fondo navy, icono 🎉 con animación pop
- Título: "Genial, este es el inicio de grandes cosas"
- Mensaje principal: correo enviado para activar la cuenta
- Muestra el correo registrado
- **Reenviar correo**: deshabilitado con countdown 2:00 (se habilita al llegar a 0, reinicia al reenviar)
- **Modificar mi correo**: vuelve al formulario con foco en el campo de correo
- **Ver todos los planes**: → `planes.html`

---

## onboarding.html — Flujo Mi Cuenta (usuarios con sesión)

Flujo de 4 pasos con stepper. Usuario simulado: **Restaurante Josefina** (POS1235456).
Acceso: botón "Iniciar sesión" en navbars.

### Paso 1 — Mi suscripción
- Alerta de vencimiento próximo (14 días)
- Plan actual: Emprendedor 3000 Silver Pro
- Métricas de uso: facturas, comprobantes, usuarios (barras de progreso)
- Método de pago registrado: Visa ···· 4242

### Paso 2 — Elegir plan
- Toggle mensual / anual (descuento 20%)
- Grid de 4 planes con selección interactiva

### Paso 3 — Resumen y pago
- 3 métodos de pago + order summary con IVA 19%

### Paso 4 — Confirmación
- Éxito con botones: Ir a mi suscripción / Descargar comprobante

---

## Decisiones de diseño tomadas

- **Tabs de precios en landing**: `<button>` con JS `switchPlanTab()` que opera sobre `#precios .plan-product-panel` — no navegan a otras páginas.
- **Toggle trimestral/anual**: CSS puro — clase `period-anual` en `#precios` controla `.price-trim` / `.price-anual` con `!important` para ganar al `display:flex` de `.plan-price`.
- **No existe opción mensual** en las landing pages. El periodo base es trimestral.
- **Precios de planes** en `planes.html` son en `/mes` (no trimestrales) porque es el flujo de compra real.
- **Modal de restaurantes** en `index.html`: se abre a los 0.8s, se cierra al hacer clic en el backdrop.
- **Botón fijo "Quiero mi POS"** en landings: esquina inferior derecha, animación CSS de pulso.
- **Botón fijo "Empieza gratis hoy"**: top-center, estilo cápsula que emerge del navbar (`border-radius: 0 0 50px 50px`), solo en `recomendar.html` y `planes.html`.
- **`facturacion.html`** es intencionalmente minimalista — pantalla de espera mientras se diseña el producto.
- **`recomendar.html`** es el entry point de todos los CTAs principales de las landing pages.
- **Sesión simulada en `prueba-gratis.html`**: el modal de "email ya existe" simula que el sistema detecta una cuenta previa — la primera interacción muestra el modal, la segunda procede.
- El proyecto se despliega en **GitHub Pages** con `index.html` como entry point.

---

## Estado actual

| Página              | Estado       | Notas                                              |
|---------------------|-------------|-----------------------------------------------------|
| `index.html`        | ✅ Completo  | Landing + modal restaurantes + pricing con tabs     |
| `restaurantes.html` | ✅ Completo  | Landing especializada + pricing con tabs            |
| `facturacion.html`  | ✅ Completo  | Pantalla "en diseño"                                |
| `recomendar.html`   | ✅ Completo  | Split 50/50: quiz guiado + self-serve               |
| `planes.html`       | ✅ Completo  | 3 pasos + planes configurables con precio dinámico  |
| `prueba-gratis.html`| ✅ Completo  | Formulario + modal + confirmación con timer         |
| `onboarding.html`   | ✅ Completo  | Mi cuenta: 4 pasos para usuarios con sesión         |

**Próximo paso pendiente**: diseñar el contenido completo de `POS Facturación electrónica Lite` para reemplazar la pantalla de espera.
