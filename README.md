```markdown
# Trucha Arcoíris — Infografía Interactiva

> Visualización de datos sobre el crecimiento de *Oncorhynchus mykiss* en jaulas flotantes en el Lago Titicaca, zona de Chucuito, Puno.

---

## Vista previa

La infografía presenta cinco indicadores clave extraídos de la investigación de Mamani & Choque (2018), con un diseño de estilo dashboard que combina datos cuantitativos con contexto cualitativo revelado interactivamente.

**Indicadores visualizados:**

| Indicador | Valor | Contexto |
|---|---|---|
| Tasa de Supervivencia | 98.40 % | Éxito en jaulas flotantes |
| Ganancia de Peso | 172.93 g | Desde 38.83 g hasta 211.76 g |
| Crecimiento en Talla | 23.94 cm | Longitud promedio alcanzada |
| Factor de Condición (K) | 1.54 | Bienestar y estado nutricional óptimo |
| Temperatura Promedio | 12.42 °C | Temperatura del agua en Chucuito |

---

## Estructura del proyecto

```
infografia-trucha/
├── index.html          # Archivo único con toda la infografía
└── README.md           # Este documento
```

El proyecto es **monolítico por diseño**: todo el markup, los estilos y la lógica de interactividad residen en `index.html`. No existe paso de compilación, no hay dependencias locales que instalar y no se generan archivos auxiliares.

### Arquitectura interna de `index.html`

El archivo se organiza en cuatro bloques secuenciales:

```
<head>
├── Meta etiquetas (charset, viewport)
├── CDN externos (Tailwind, Font Awesome, Google Fonts)
├── Configuración extendida de Tailwind (colores, tipografías)
└── <style> — Estilos CSS propios
    ├── Variables de diseño (:root)
    ├── Fondo atmosférico (gradientes, partículas)
    ├── Sistema de animaciones (reveal, shimmer, float)
    ├── Componentes (tarjetas, barras, gráfico SVG)
    ├── Estados interactivos (hover, focus)
    ├── Responsividad (@media queries)
    └── Accesibilidad (prefers-reduced-motion)

<body>
├── .bg-scene           # Capa de fondo con partículas (JS)
├── <main>
│   ├── <header>        # Título, nombre científico, badge de ubicación
│   ├── <section>       # Grid de 5 tarjetas de datos
│   │   ├── Tarjeta 01: Supervivencia (SVG personalizado + barra)
│   │   ├── Tarjeta 02: Ganancia de Peso (rango inicial/final)
│   │   ├── Tarjeta 03: Crecimiento en Talla (barras visuales)
│   │   ├── Tarjeta 04: Factor de Condición (escala con marcador)
│   │   └── Tarjeta 05: Temperatura (SVG personalizado + gráfico SVG)
│   ├── Separador de onda (decorativo)
│   ├── <section>       # Resumen del estudio + tags
│   ├── <section>       # Metodología (3 pasos)
│   └── <footer>        # Citas APA 7 + créditos
├── .toast              # Notificación de bienvenida
└── <script>            # Lógica JavaScript
    ├── Generador de partículas
    ├── IntersectionObserver (animaciones de entrada)
    ├── Contadores numéricos animados
    ├── Control del toast
    └── Parallax del header
```

---

## Tecnologías utilizadas

### Dependencias externas (CDN)

| Tecnología | Versión | Rol en el proyecto |
|---|---|---|
| **Tailwind CSS** | 3.x (CDN) | Framework de utilidades CSS para layout, espaciado, tipografía y responsive |
| **Font Awesome** | 6.5.0 | Iconografía en tarjetas, badges, metodología y footer |
| **Google Fonts** | — | Tipografías Playfair Display (títulos) y DM Sans (cuerpo) |

### HTML y CSS nativo

- **CSS Custom Properties** (`:root`): Sistema centralizado de color (azules lacustres, plateados metálicos, coral), aplicado en toda la hoja de estilos.
- **SVG inline**: Dos iconos personalizados (Supervivencia y Temperatura) embebidos directamente; un gráfico de evolución del peso con animación de trazo (`stroke-dashoffset`).
- **Backdrop-filter**: Efecto de vidrio esmerilado en tarjetas (`blur(16px)`).
- **CSS Keyframes**: Animaciones de brillo plateado (`shimmer`), flotación de partículas (`floatUp`), y deriva de gradientes de fondo (`drift1`, `drift2`).

### JavaScript (vanilla, sin dependencias)

| Funcionalidad | API / técnica | Descripción |
|---|---|---|
| Animaciones de entrada | `IntersectionObserver` | Detecta cuándo cada elemento con clase `.reveal` entra al viewport y activa transiciones CSS |
| Contadores numéricos | `requestAnimationFrame` | Anima valores de 0 al objetivo con easing cúbico (`1 - (1-t)³`) |
| Partículas flotantes | `document.createElement` | Genera entre 15 y 30 elementos `div` con posiciones y duraciones aleatorias |
| Parallax del header | `scroll` + `requestAnimationFrame` | Desplazamiento vertical y atenuación de opacidad al hacer scroll |
| Toast de bienvenida | `IntersectionObserver` + `setTimeout` | Se muestra al detectar la primera tarjeta; se auto-oculta a los 9.5 s |
| Activación del gráfico SVG | Manipulación de clases | Añade `.animate` a línea, puntos y etiquetas del gráfico de peso |

---

## Cómo visualizar localmente

### Opción 1: Abrir directamente

Doble clic sobre `index.html`. Se abre en el navegador predeterminado. Todas las dependencias se cargan desde CDN, por lo que se requiere conexión a internet.

### Opción 2: Servidor local (recomendado)

Un servidor local evita restricciones CORS y mejora la recarga. Cualquiera de estas opciones funciona:

```bash
# Con Python 3
python -m http.server 8080

# Con Node.js (npx, sin instalación global)
npx serve .

# Con PHP
php -S localhost:8080
```

Luego abrir `http://localhost:8080` en el navegador.

### Opción 3: Live Server (VS Code)

1. Instalar la extensión **Live Server** desde el marketplace.
2. Clic derecho sobre `index.html` → **Open with Live Server**.
3. Se abre en `http://127.0.0.1:5500` con recarga automática.

---

## Paleta de colores

Los colores están definidos como variables CSS en `:root` y extendidos en la configuración de Tailwind:

| Variable | Hex | Uso |
|---|---|---|
| `--bg-deep` | `#020a18` | Fondo profundo del body |
| `--bg` | `#061628` | Fondo de tarjetas y secciones |
| `--card` | `rgba(12, 34, 64, 0.55)` | Superficie de tarjetas (glassmorphism) |
| `--accent` | `#e85d6f` | Coral: iconos, barras, bordes hover, énfasis |
| `--fg` | `#eaf0f6` | Texto principal |
| `--fg-muted` | `#7890a8` | Texto secundario y etiquetas |
| `--silver-shine` | `rgba(200, 214, 229, 0.15)` | Partículas y reflejo metálico en valores |

---

## Tipografías

| Fuente | Peso | Uso |
|---|---|---|
| **Playfair Display** | 400, 700, 900 | Título principal, valores numéricos (gradiente plateado), subtítulos de sección |
| **DM Sans** | 300–700 | Cuerpo de texto, etiquetas, contexto, footer, badges |

---

## Interactividad

- **Hover en tarjetas**: Elevación (`translateY(-6px)`), borde coral, resplandor difuso, reflejo metálico deslizante, y revelado del párrafo de contexto con transición `max-height`.
- **Gráfico SVG**: La curva de evolución del peso se dibuja progresivamente al entrar al viewport. Los puntos y etiquetas aparecen escalonadamente.
- **Contadores**: Cada valor numérico se anima desde 0 hasta su valor final con easing cúbico.
- **Parallax**: El header se desplaza y atenúa proporcionalmente al scroll.

---

## Accesibilidad

- Estructura semántica con `<header>`, `<main>`, `<section>`, `<article>`, `<footer>`.
- Atributos `aria-label` en todas las secciones y el gráfico SVG.
- `role="img"` y `aria-label` descriptivo en el SVG de evolución del peso.
- `role="alert"` y `aria-live="polite"` en el toast.
- `aria-label` en el botón de cierre del toast.
- `prefers-reduced-motion: reduce` desactiva todas las animaciones y oculta partículas.
- Scrollbar personalizada con contraste suficiente sobre el fondo oscuro.

---

## Referencias bibliográficas

> Mamani, J., & Choque, A. (2018). Crecimiento de la trucha arcoíris (*Oncorhynchus mykiss*) en jaulas flotantes en la zona de Chucuito, Puno.

> Horna, V. y equipo. (2026). Brief de diseño para la infografía interactiva: Proyecto Trucha.

---

## Licencia

Este proyecto fue desarrollado con fines académicos. Los datos pertenecen a los autores de la investigación citada.
```