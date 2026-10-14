# gasykilometros

Página personal para desarrollo y autoaprendizaje de contenido sintético con IA. Sitio estático sin ánimo de lucro, centrado en el mundo de la moto.

**Producción:** https://gasykilometros.netlify.app

---

## Qué es

`gasykilometros` es un sitio web estático (HTML + CSS, sin frameworks ni build) que sirve de cuaderno de bitácora personal sobre motos: novedades del sector, guías de mantenimiento, una colección de fichas técnicas propias y un canal de comentarios. Todo el contenido es sintético generado con IA y revisado manualmente.

## Estructura del sitio

El sitio se articula en **5 secciones**, accesibles desde un menú común presente en todas las páginas:

| Sección | Archivo | Contenido |
|---|---|---|
| **Inicio** | `index.html` | Portada con tarjetas visuales a las demás secciones |
| **Novedades** | `novedades.html` | Entradas tipo blog sobre el sector (las más recientes primero) |
| **Taller** | `taller.html` | Guías de mantenimiento y consumibles de motos |
| **Mi garage** | `garage.html` | Fichas técnicas de las motos propias, con fotografía y referencias |
| **Comentarios** | `comentarios.html` | Formulario de feedback (correo obligatorio pero oculto al público) |

### Índice de archivos

```
index.html                          Portada / landing
novedades.html                      Índice de entradas de Novedades
novedades-zontes-552h.html          Entrada: Zontes 552H (EICMA 2025)
novedades-voge-sr450x.html          Entrada: Voge SR450X (crossover bicilíndrico A2)
novedades-moto-guzzi-v85tt.html     Entrada: Moto Guzzi V85 TT (trail media con cardán)
taller.html                         Índice de guías de Taller
taller-beverly-350.html             Guía: Piaggio Beverly 350 Sport Touring
taller-maxsym-tl-508.html           Guía: SYM Maxsym TL 508
taller-bmw-r1200gs.html             Guía: BMW R 1200 GS (2011)
taller-interceptor-650.html         Guía: Royal Enfield Interceptor 650 (dinámica y tuning performance)
garage.html                         Mi garage: 7 fichas técnicas de motos
comentarios.html                    Formulario de comentarios (Netlify Forms)
comentarios-exito.html              Confirmación tras enviar un comentario
404.html                            Página de error personalizada
estilos.css                         Hoja de estilos única de todo el sitio
netlify.toml                        Configuración de despliegue Netlify
```

### Novedades y Taller: sistema de entradas

Ambas secciones usan el mismo patrón de blog:

- **`novedades.html`** y **`taller.html`** son índices que listan tarjetas (`article.entrada`) con **miniatura + fecha + título + resumen**.
- Cada entrada es un **archivo HTML independiente** que sigue una plantilla común: cabecera hero, ficha técnica en tablas, secciones de análisis, avisos y fuentes.
- **Las entradas nuevas se insertan al principio** del índice (las más recientes arriba), marcado con un comentario en el HTML.
- Las miniaturas viven en `img/` con prefijo `mini-*` y se generan a 600 px (recorte 4:3).

### Mi garage

`garage.html` es la sección más elaborada: contiene **7 fichas técnicas** (de un Derbi FDS de 1987 a un SYM Maxsym TL 508 de 2024), cada una con fotografía, ficha técnica plegable (`<details>`), balance de pros/contras, referencias cruzadas y una tabla de créditos de imagen con licencias.

## Despliegue (Netlify)

El sitio se publica en Netlify como **estático puro**:

```toml
# netlify.toml
[build]
  publish = "."
```

- No hay paso de build: se sirve el directorio raíz directamente.
- **Formulario de comentarios**: usa **Netlify Forms** (`data-netlify="true"`). Los envíos se gestionan desde el panel de Netlify (solo el propietario ve los correos). El formulario debe estar registrado en el deploy (si se desactiva Forms, el envío devuelve 404).
- Despliegue en local para revisión: `python3 -m http.server 8181` (o `netlify dev`).

## Estructura de imágenes

```
img/                  Imágenes compartidas: miniaturas de sección y de entradas
img/garaje/           Fotografías de las motos del garage
portada.png           Imagen de portada del index
```

Las imágenes de terceros se descargan localmente y se **atribuyen** (autor + licencia) en el pie de foto o en la tabla de créditos de `garage.html`.

## Tecnología

- **HTML5 + CSS3** (variables CSS, grid/flexbox, media queries, diseño responsive).
- Sin JavaScript en el front (solo el que inyecta Netlify para los formularios).
- Sin dependencias ni gestor de paquetes.

## Estilo y convenciones

- Idioma del contenido: **español** (texto acentuado y corregido).
- `lang="es"` en todas las páginas, con `charset UTF-8` y `viewport`.
- Un único `<h1>` por página; estructura semántica (`header`, `main`, `footer`, `article`, `figure`).
- Navegación común en `<header>` con el enlace activo marcado (`class="active"`).
- Tablas para fichas técnicas; avisos resaltados con `p.aviso`.
- Nota legal de copyright en el pie: si alguna referencia o imagen vulnera derechos, se pide feedback y se subsana.

## Mantenimiento

- **Añadir una entrada de Novedades/Taller**: crear el HTML siguiendo la plantilla existente, generar una miniatura en `img/mini-*.jpg` e insertar la tarjeta al principio del índice.
- **Corrección ortográfica**: al editar texto, mantener los acentos y la `ñ` conforme al castellano.
- **Imágenes**: descargar localmente y citar autor + licencia.
