# Ranking Online · Component Library

Librería HTML/CSS con secciones y screens extraídos de los proyectos del workspace `ranking_online`, refactorizados sobre un único sistema de diseño.

## Estructura

```
librery_components-ranking_online/
├── index.html                # Showcase + índice navegable
├── assets/
│   └── css/
│       ├── base.css          # Único import por archivo
│       ├── tokens.css        # Variables: paleta, espacios, radios, sombras
│       ├── typography.css    # 4 sets tipográficos + escala fluida
│       ├── reset.css         # Reset moderno
│       └── utilities.css     # .btn, .container, .section, .chip, grid…
├── sections/                 # Secciones tipo S01–S13
│   └── S0X-nombre/
│       └── cliente/          # ej. sol4, medalva, triplex…
│           ├── index.html
│           └── styles.css
├── components/               # Componentes atómicos reusables
└── screens/                  # Composición de varias secciones en una página
```

## Cómo añadir una sección

1. Crear carpeta `sections/SXX-nombre/cliente/`.
2. En el `<html>`, cargar la base:
   ```html
   <link rel="stylesheet" href="../../../assets/css/base.css" />
   <link rel="stylesheet" href="./styles.css" />
   ```
3. Aplicar el tema y el set tipográfico en la `<section>` o en `<body>`:
   ```html
   <body class="font-set-2">
     <section class="section theme-crimson">…</section>
   </body>
   ```
4. Reutilizar utilidades antes de escribir CSS nuevo (`.container`, `.section`, `.btn-primary`, `.chip`, `.grid-cols-3`…).
5. Sólo escribir CSS específico para lo que sea único de la sección.

## Sistema de temas

Aplicá una clase `theme-*` a cualquier `<section>` para sobreescribir las variables `--brand-50` a `--brand-900`:

| Clase | Hue | Uso típico |
|---|---|---|
| `theme-purple`  | Lila | Sol-4, Christian Sánchez |
| `theme-crimson` | Burdeos | Medalva |
| `theme-indigo`  | Azul profundo | Consulting-F |
| `theme-steel`   | Azul acero | Tribulex shell, Aselegal |
| `theme-gold`    | Dorado | Tribulex acento, elegancia |
| `theme-teal`    | Teal | Ecom Advisory, Gestiona |
| `theme-forest`  | Verde | Segu Web |

## Sets tipográficos

| Clase | Familias | Uso |
|---|---|---|
| `font-set-1` | DM Sans + Inter | Moderno neutral |
| `font-set-2` | Playfair + Inter | Elegante editorial (legal) |
| `font-set-3` | Nunito Sans | Humanista cálido |
| `font-set-4` | Space Grotesk + IBM Plex | Display contemporáneo |

## Referencia

- Documento maestro: `Seleccion_Secciones_Templates_RankingOnline`
- Workspace de origen (solo lectura): `/Users/francisco.v98/projects/ranking_online/`
