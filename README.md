# IxDF Mendoza · Comunidad

Sitio del grupo local de la [Interaction Design Foundation](https://ixdf.org/local-group/south-america/argentina/mendoza?r=guadalupe-5) en Mendoza: un espacio para aprender, compartir y crecer en diseño, encuentro a encuentro.

Se hizo en vivo en el workshop **Del Diseño a la Web con Claude** (21/11), llevando un diseño de Figma a una web publicada con Figma MCP, Claude Code, GitHub y Vercel. El archivo de diseño está en la [Comunidad de Figma](https://www.figma.com/community/file/1705769957340413540).

## Qué tiene

- **Hero**: presentación del grupo, botón al grupo de WhatsApp y datos rápidos (encuentros, asistentes, workshops).
- **Encuentros**: una card por encuentro. "Ver detalle" abre un modal con la actividad, las fotos (o el banner) y los links y materiales.
- **Próximos temas**: torta "explotada" en SVG con los temas del card sorting, su leyenda y un acceso a la encuesta.
- **Footer** con redes y la página del grupo en IxDF.

## Stack

Un solo `index.html` con el CSS y el JS adentro. Sin frameworks, sin librerías y sin paso de build. Las fuentes vienen de Google Fonts (Bricolage Grotesque, Inter, JetBrains Mono y Space Grotesk).

```
.
├── index.html     # estructura, estilos, contenido y comportamiento
└── assets/        # portadas, fotos de los encuentros y banner del workshop
```

## Verlo en local

Abrí `index.html` en el navegador o levantá un servidor estático:

```bash
python3 -m http.server 8000
```

y entrá a http://localhost:8000.

## Editar el contenido

Todo el contenido está en arrays al principio del `<script>`, así que no hace falta tocar el HTML:

| Constante | Qué contiene |
|---|---|
| `sitio` | Bajada del hero, links de WhatsApp y de la encuesta, menú, datos del hero y redes del footer |
| `encuentros` | Un objeto por encuentro: fecha, etiqueta, título, resumen, portada, actividad, fotos o banner, detalles, temario y links |
| `temas` | Temas del card sorting y cantidad de ideas. El gráfico, los porcentajes y la leyenda se calculan desde acá |

Para sumar un encuentro, agregá un objeto a `encuentros` con un `id` único y poné sus imágenes en `assets/`. La card y el modal se generan solos.

Un link con `url: ""` o con `pendiente: true` se muestra en gris y no se puede tocar. Sirve, por ejemplo, para la grabación antes de que esté publicada.

## Design system

Las variables de Figma están pasadas a variables CSS con el mismo nombre (`color/bg` → `--color-bg`, `radius/lg` → `--radius-lg`, `border/width` → `--border-width`…). Hay dos colecciones, y cada una se elige con un atributo en `<html>`:

| Colección | Atributo | Modos |
|---|---|---|
| Paleta | `data-paleta` | `Taller`, `Bosque`, `Noche`, `Mono` |
| Estilo | `data-estilo` | `Neobrutalista`, `Minimal`, `Techy`, `Notion` |

Por defecto el sitio usa **Taller + Techy**, como en el diseño. Para probar otra combinación sin editar nada, agregá parámetros a la URL:

```
index.html?paleta=Noche&estilo=Neobrutalista
```

El fondo cambia según el estilo: cuadrícula inclinada −12° en Neobrutalista, degradé en Techy y liso en Minimal y Notion.

Los estilos de texto de Figma están como clases: `.t-display`, `.t-titulo`, `.t-subtitulo`, `.t-cuerpo` y `.t-etiqueta`. Las medidas fijas del archivo (espaciados, anchos, altos de imagen) también están declaradas como variables en `:root`.

### Componentes

`Boton` (primario, secundario o fantasma; grande o chico), `Chip`, `Card`, `ItemLeyenda`, `FilaLink`, `Modal` y `Grafico`. Son funciones de JS que devuelven HTML. El nav superior, los encabezados de sección y el footer están en el HTML.

## Responsive

- **Desktop (1024 px o más):** las tres secciones tienen el mismo alto, como en Figma.
- **Tablet:** el hero se apila y los datos pasan a una fila.
- **Menos de 900 px:** el menú se vuelve hamburguesa.
- **Celular (menos de 640 px):** todo va en una columna y la leyenda queda debajo del gráfico.
- **Cards:** se desplazan en horizontal cuando no entran.

## Accesibilidad

- HTML semántico, con un link para saltar al contenido.
- Texto alternativo en todas las imágenes.
- Foco visible en todos los elementos interactivos.
- El modal usa `<dialog>`: se cierra con la cruz, con Escape o tocando afuera, y al cerrarse el foco vuelve al botón que lo abrió.
- Los botones que llevan a otro sitio avisan a los lectores de pantalla que se abren en otra pestaña.
- El scroll suave se desactiva con `prefers-reduced-motion`.

## Assets

| Archivo | Uso | Tamaño |
|---|---|---|
| `portada-brunch.jpg` | Portada de la card Brunch & Diseño | 1080 × 1350 |
| `portada-picnic.jpg` | Portada de la card Picnic de Diseño | 1179 × 2096 |
| `portada-workshop.jpg` | Portada de la card Workshop de IA | 1080 × 1920 |
| `brunch-1.jpg` … `brunch-4.jpg` | Fotos del modal Brunch & Diseño | 640 × 440 |
| `picnic-1.jpg` … `picnic-4.jpg` | Fotos del modal Picnic de Diseño | 640 × 440 |
| `banner-workshop.jpg` | Banner del modal Workshop de IA | 2048 × 768 |

Las portadas se muestran con `object-fit: cover` a 250 px de alto. El encuadre de cada una se ajusta con la propiedad `foco` del encuentro. Las fotos de los modales van en un carrusel horizontal, a 260 px de alto y sin recortar.

## Publicar

Es un sitio estático, así que se puede publicar tal cual en Vercel, Netlify o GitHub Pages. En Vercel: importá el repo, elegí el preset **Other** y dejá vacíos los comandos de build.
