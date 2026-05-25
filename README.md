# Ranking Online · Component Library

Librería HTML/CSS con **39 secciones**, **3 templates** y un **sistema de diseño unificado** extraídos de 11 proyectos reales de Ranking Online y refactorizados como bloques de lego intercambiables.

- **0 JavaScript.** Todo (carruseles, tabs, FAQs, modales) está resuelto con CSS puro.
- **0 dependencias externas.** Sin fuentes auto-cargadas, sin frameworks, sin CDNs obligatorios.
- **Cualquier sección encaja con cualquier otra.** Mismo set de tokens, themes intercambiables, tipografía global.

---

## Tabla de contenidos

1. [Estructura del proyecto](#estructura)
2. [Quickstart — ver la librería](#quickstart)
3. [Cómo usar una sección suelta](#usar-una-seccion)
4. [Cómo personalizar para un cliente](#personalizar)
5. [Cómo armar un template nuevo](#nuevo-template)
6. [Sistema de diseño](#sistema)
7. [Catálogo completo de secciones](#catalogo)
8. [Templates incluidos](#templates)
9. [Info-panel por sección](#info-panel)

---

<a id="estructura"></a>
## 1. Estructura del proyecto

```
librery_components-ranking_online/
├── index.html                  # Dashboard navegable (paleta, tipografía, catálogo, templates)
├── README.md
│
├── assets/
│   ├── css/
│   │   ├── base.css            # Único entry point (importa tokens + typography + reset + utilities)
│   │   ├── tokens.css          # Variables: paleta, espacios, radios, sombras, motion
│   │   ├── typography.css      # 4 sets tipográficos + escala fluida con clamp()
│   │   ├── reset.css           # Reset moderno
│   │   ├── utilities.css       # .btn, .container, .section, .chip, helpers de grid
│   │   └── info-panel.css      # Botón flotante + slide-in panel (CSS-only)
│   ├── images/                 # Assets por cliente
│   └── video/                  # Vídeos por cliente
│
├── sections/                   # 14 grupos × 1–4 variantes = 39 secciones
│   └── SXX-grupo/
│       └── cliente-variante/
│           ├── index.html      # Demo standalone (cargable con file://)
│           └── styles.css      # CSS scopeado con prefijo
│
└── templates/                  # 3 composiciones de referencia
    ├── index.html              # Hub navegable
    ├── template-a/index.html   # Servicio comercial (conversión)   · 12 bloques
    ├── template-b/index.html   # Servicio corporativo (autoridad)  · 11 bloques
    └── template-c/index.html   # Landing de campaña (cold traffic) ·  7 bloques
```

---

<a id="quickstart"></a>
## 2. Quickstart — ver la librería

Abrí `index.html` con doble click (no necesita servidor):

```bash
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

Desde el dashboard navegás a:

- **01 · Catálogo** — secciones agrupadas por cliente o por grupo (tabs CSS-only).
- **02 · Paleta** — 7 escalas de color con todos los tonos 50–900.
- **03 · Tipografía** — 4 sets tipográficos con muestra.
- **04 · Templates** — 3 templates listos para clonar.

> Si tu navegador bloquea fuentes/imágenes locales por CORS, levantá un server estático: `python3 -m http.server`.

---

<a id="usar-una-seccion"></a>
## 3. Cómo usar una sección suelta

Cada sección es **self-contained**: un `index.html` que importa `base.css` global + su `styles.css` local. Para usarla en otro proyecto, copiá la carpeta + ajustá las rutas relativas.

```html
<!doctype html>
<html lang="es">
  <head>
    <link rel="stylesheet" href="ruta/a/assets/css/base.css" />
    <link rel="stylesheet" href="ruta/a/sections/S01-hero/medalva/styles.css" />
  </head>
  <body class="font-set-3">
    <section class="hero-medalva theme-crimson">
      <!-- contenido de la sección -->
    </section>
  </body>
</html>
```

**Las dos clases clave:**

- `font-set-X` en `<body>` define la familia tipográfica global de la página.
- `theme-X` en cada `<section>` define el set de variables `--brand-*` (50–900) que usa esa sección.

Podés tener un body con `font-set-1` (DM Sans) y secciones con themes distintos (`theme-purple`, `theme-teal`, `theme-gold`) conviviendo sin conflictos.

---

<a id="personalizar"></a>
## 4. Cómo personalizar para un cliente

La capa de personalización tiene **5 niveles**, de menos a más invasivo:

### 4.1 Cambiar tema de color (1 línea)

Reemplazá la clase `theme-X` en la `<section>`. Eso re-mapea automáticamente `--brand-50` a `--brand-900`:

```html
<!-- antes -->
<section class="hero-medalva theme-crimson">

<!-- después → mismo hero, paleta púrpura -->
<section class="hero-medalva theme-purple">
```

### 4.2 Cambiar tipografía (1 línea)

Reemplazá `font-set-X` en `<body>` o en una `<section>` puntual:

```html
<body class="font-set-2">  <!-- Playfair + Inter, editorial -->
```

### 4.3 Cambiar copy

Todos los textos están **inline en el HTML** (sin fetch, sin i18n). Editá H1, leads, CTAs, bullets, FAQs… directamente.

### 4.4 Cambiar imágenes / vídeos / iconos

- **Imágenes:** reemplazá los archivos en `assets/images/<cliente>/` manteniendo nombres y aspect ratio.
- **Vídeos:** mismo proceso en `assets/video/<cliente>/`.
- **Iconos:** son SVG inline. Reemplazá el `<path d="…">` manteniendo el `viewBox` y `stroke-width`.

### 4.5 Tokens globales (cambios cross-section)

Si el cliente necesita ajustes globales (espaciado más generoso, radios más cuadrados, sombras más planas) editá `assets/css/tokens.css`:

```css
:root {
  --space-4: 1.25rem;   /* ej. aumentar densidad */
  --radius-md: 4px;     /* ej. UI más cuadrada */
  --shadow-md: none;    /* ej. flat design */
}
```

> El **info-panel flotante** dentro de cada sección lista exactamente qué se puede personalizar en esa variante específica. Buscá el botón "i" en la esquina inferior derecha cuando estés viendo una sección.

---

<a id="nuevo-template"></a>
## 5. Cómo armar un template nuevo

Un template es simplemente un **único `index.html`** que importa los CSS de las secciones elegidas y embebe el `<body>` de cada una en orden.

### 5.1 Anatomía de un template (4 capas)

```
templates/template-X/
├── index.html          ← Estructura: header + secciones + footer
└── unify.css           ← Overrides por sección para armonía visual

assets/css/
├── base.css            ← Tokens + tipografía + reset + utilities (común a todo)
└── template-shell.css  ← Header + footer estandarizados (común a los 3 templates)
```

Cada template aplica **un único `theme-*` y un único `font-set-*`** a todos sus bloques, garantizando armonía visual completa.

### 5.2 Estructura mínima

```html
<!doctype html>
<html lang="es">
<head>
  <link rel="stylesheet" href="../../assets/css/base.css" />

  <!-- Un <link> por sección que vayas a usar -->
  <link rel="stylesheet" href="../../sections/S01-hero/medalva-form/styles.css" />
  <link rel="stylesheet" href="../../sections/S05-proceso/sol4/styles.css" />
  <!-- … -->

  <!-- Shell común (header + footer) y unify específico del template -->
  <link rel="stylesheet" href="../../assets/css/template-shell.css" />
  <link rel="stylesheet" href="./unify.css" />
</head>

<!-- 3 clases en el body: font-set + tpl-body + theme único -->
<body class="font-set-3 tpl-body theme-crimson">

  <!-- HEADER común (importa estilos de template-shell.css) -->
  <header class="t-header"> … </header>

  <!-- Secciones — TODAS con el mismo theme-X que el body -->
  <section class="herof-medalva theme-crimson"> … </section>
  <section class="clarity-eca theme-crimson"> … </section>
  <!-- … -->

  <!-- FOOTER común -->
  <footer class="t-footer"> … </footer>

</body>
</html>
```

### 5.3 El archivo `unify.css` (clave para la armonía)

Cada sección de la librería trae su propia identidad heredada del cliente original (gradients hardcoded, variables internas como `--eca-accent`, `--rm-accent`, `--bd-green`…). Cuando se mezclan en un template aparecerían colores fuera de paleta.

El `unify.css` resuelve esto sobrescribiendo:

- **Variables locales** de cada sección (`--eca-accent`, `--gestiona-primary`, `--sgu-lime`, etc.) mapeándolas a `var(--brand-*)`.
- **Backgrounds con HEX hardcoded** (gradients de heroes, fondos crema, etc.) mapeándolos al theme.
- **Variables creme/neutral** que tenían fondos específicos → neutrales de la librería.

Ejemplo de override en `template-a/unify.css`:

```css
/* La sección clarity-eca usa --eca-accent: var(--teal-300) por defecto.
   En Template A (theme-crimson) la remapeamos al brand del template */
.tpl-body .clarity-eca {
  --eca-accent: var(--brand-300);
  background: var(--surface-warm);
}
```

### 5.4 Workflow para armar un template nuevo

1. **Definí caso de uso**: buyer persona, objetivo de conversión, longitud.
2. **Elegí theme + font-set único** para todo el template.
3. **Elegí grupo + variante** de cada bloque desde el catálogo.
4. **Creá la carpeta** `templates/<nombre>/index.html` + `unify.css` (vacío).
5. **Importá** `base.css` + un `<link>` por sección + `template-shell.css` + `./unify.css`.
6. **Body** con 3 clases: `font-set-X tpl-body theme-X`.
7. **Insertá** header (`<header class="t-header">`) + secciones (forzando el mismo `theme-X` en cada una) + footer (`<footer class="t-footer">`).
8. **Auditá overrides**: identificá variables internas y HEX hardcoded por sección y mapeáles overrides en `unify.css`.

### 5.5 Composición programática

El proyecto incluye un script Python (no versionado en repo) que automatiza los pasos 5–7: lee la lista `[(ruta_seccion, label), ...]`, reemplaza todas las clases `theme-*` por el theme elegido y ensambla `index.html` con header + bodies + footer. Los 3 templates incluidos fueron generados con este script.

---

<a id="sistema"></a>
## 6. Sistema de diseño

### 6.1 Themes de color (7)

Aplicá `theme-*` a una `<section>` para sobrescribir `--brand-50` a `--brand-900`:

| Clase | Hue | Uso típico |
|---|---|---|
| `theme-purple`  | Lila profundo | Sol-4, Christian Sánchez, Ecom Advisory |
| `theme-crimson` | Burdeos | Medalva |
| `theme-indigo`  | Azul profundo | Consulting-F, Aselegal, Romero Martínez |
| `theme-steel`   | Azul acero | Tribulex shell, formal |
| `theme-gold`    | Dorado | Acento de elegancia |
| `theme-teal`    | Teal | Gestiona Asesoría, financial-tech |
| `theme-forest`  | Verde profundo | Segu Web, sustentable |

### 6.2 Sets tipográficos (4)

Aplicá `font-set-*` a `<body>`:

| Clase | Familias | Uso |
|---|---|---|
| `font-set-1` | DM Sans + Inter | Moderno neutral · tech / consultoría |
| `font-set-2` | Playfair + Inter | Elegante editorial · legal / autoridad |
| `font-set-3` | Nunito Sans | Humanista cálido · asesoría humanista |
| `font-set-4` | Space Grotesk + IBM Plex | Display contemporáneo · tech-forward |

### 6.3 Tokens disponibles

Ver `assets/css/tokens.css`. Los más usados:

```
--space-1 … --space-20      Escala de espaciado
--radius-sm/md/lg/xl/pill   Radios de borde
--shadow-sm/md/lg/xl        Sombras
--duration-fast/base/slow   Motion
--ease-out, --ease-in-out   Easings
--fs-micro/caption/body-sm/body/lead/h4/h3/h2/h1/hero  Escala tipográfica
```

---

<a id="catalogo"></a>
## 7. Catálogo completo de secciones

14 grupos × variantes:

| Código | Grupo | Variantes |
|---|---|---|
| **S01** | Hero | sol4, christian-sanchez, medalva, **medalva-form** |
| **S02** | Problemática | ecom-advisory, romero-martinez, gestiona-asesoria |
| **S03** | Para quién es este servicio | medalva, aselegal, tribulex |
| **S04** | Qué incluye el servicio | tribulex |
| **S05** | Proceso de trabajo | sol4, medalva, ecom-advisory |
| **S06** | Beneficios / Lo que consigues | sol4, ecom-advisory, consulting-f |
| **S07** | Tabla comparativa Problema / Solución | consulting-f, segu-web, barreda-diaz |
| **S08** | Servicios relacionados / Cross-sell | sol4, christian-sanchez, medalva |
| **S09** | Formulario de contacto | sol4, ecom-advisory, aselegal |
| **S10** | Mapa | tribulex, consulting-f, segu-web |
| **S11** | Reviews | aselegal, tribulex, barreda-diaz |
| **S12** | FAQs | christian-sanchez, aselegal, segu-web |
| **S13** | Cierre / CTA final | medalva, romero-martinez, tribulex |
| **S14** | Enfoque diferencial + form *(nuevo)* | standard |

**Total: 39 secciones.**

---

<a id="templates"></a>
## 8. Templates incluidos

Cada template aplica **un único theme + un único font-set** a todos sus bloques (armonía visual completa) y trae **header sticky + footer** estandarizados.

| Template | Caso de uso | Brand demo | Bloques | Theme unificado | Tipografía |
|---|---|---|---|---|---|
| [Template A](templates/template-a/index.html) | Servicio comercial · conversión | Vértice Asesores | 12 + header/footer | `theme-crimson` | Set 3 · Nunito Sans |
| [Template B](templates/template-b/index.html) | Servicio corporativo · autoridad | Castilla & Sosa | 11 + header/footer | `theme-indigo` | Set 2 · Playfair + Inter |
| [Template C](templates/template-c/index.html) | Landing de campaña · cold traffic | Fiscalia.app | 7 + header/footer | `theme-teal` | Set 1 · DM Sans + Inter |

**Personalizar un template para un cliente real:**

1. Clonar la carpeta `templates/template-X/` con el nombre del cliente.
2. En `index.html`: editar las strings de brand (en header y footer), copy de cada bloque, URLs de redes sociales y datos de contacto.
3. Para cambiar el theme: reemplazar `theme-X` en `<body>` y en todas las `<section>`. Eso re-mapea automáticamente todas las variables `--brand-*`.
4. Para cambiar la tipografía: reemplazar `font-set-X` en `<body>`.
5. `unify.css` rara vez necesita tocarse — solo si el cliente pide colores muy específicos por bloque.

Ver hub navegable: [`templates/index.html`](templates/index.html).

---

<a id="info-panel"></a>
## 9. Info-panel por sección

Cada uno de los 39 `sections/SXX/.../index.html` incluye un **botón flotante "i"** en la esquina inferior derecha. Al hacer click se abre un panel slide-in con:

- **Cuándo usar esta sección** — caso de uso ideal y posición sugerida en la página.
- **Qué se puede personalizar** — checklist de tokens, copy, imágenes, iconos editables.
- **Compatibilidades** — qué otras secciones combinan especialmente bien con esta.
- **Stats técnicas** — tema, set tipográfico, assets, breakpoints, dependencias.

El panel está implementado en CSS puro (checkbox + `:checked ~`) y vive en `assets/css/info-panel.css`. Solo aparece en las páginas de demo individuales — los templates compuestos no lo cargan.

---

## Workspace de origen

Solo lectura · `/Users/francisco.v98/Projects/ranking_online/`:

- sol4-web · christian-sanchez_web-all · medalva-web · ecom_advisory-landing
- Romero_Martinez · Gestiona_Asesoria · aselega-plaza_cuba-web · tribulex-home
- Consulting-F · segu-web · Barreda_Diaz
