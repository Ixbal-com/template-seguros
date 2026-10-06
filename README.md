# Plantilla Ixbal · Agente de seguros

Plantilla para agentes y asesores de seguros independientes. Estilo **directo y confiable**: grafito y naranja señal sobre gris claro, bordes marcados y cifras grandes (títulos en Red Hat Display, texto en Red Hat Text).

**Incluye:** hero con estado "Te contesto ahora" y cédula de agente, los cuatro seguros explicados (auto, gastos médicos, vida y hogar), **cotizador en tres pasos cuyas preguntas cambian según el tipo de seguro y que llega listo por WhatsApp**, ejemplo de comparativa de tres opciones con glosario, aseguradoras con las que trabaja, **qué hacer en un siniestro** por tipo (pestañas) con llamada directa, **recordatorio de renovación** por WhatsApp, sobre mí, opiniones, preguntas frecuentes y contacto con mapa.

Ejemplo: **Rodrigo Garza**, asesor de seguros independiente en Monterrey.

## Uso

```bash
npm run dev     # servidor local en http://localhost:4321
npm run check   # valida el sitio (sin dependencias)
```

Ábrela con un servidor, no con doble clic: los navegadores no ejecutan módulos de JavaScript desde `file://`.

## Personalizar

1. **Identidad:** colores y tipografía en `assets/css/tokens.css`.
2. **Contenido:** seguros, preguntas del cotizador, aseguradoras, pasos de siniestro y horario en `index.html`, organizado por secciones.
3. **Imágenes:** 8 espacios de imagen listados en `imageSlots` de `template.json`, con fotos de ejemplo en `images/` generadas con IA (`gpt-image-2.5-sunburst`). Reemplázalas por fotos reales; ver [AGENTS.md](AGENTS.md#espacios-de-imagen).

Las reglas de arquitectura, la lista de datos que se repiten y cómo funciona el cotizador están en [AGENTS.md](AGENTS.md).

## Módulos compartidos

Los archivos de `scripts/check.mjs` y de `assets/js/modules/` (y los CSS iniciales de los módulos) vienen de la biblioteca de plantillas Ixbal (`biblioteca/`). Esta plantilla usa `tabs`, `wizard` y `whatsapp-form` (ver `"modules"` en `template.json`). Para cambiarlos en todas las plantillas, edítalos en la biblioteca y corre `npm run sync` ahí.

## Publicar

Es un sitio estático: sirve la raíz del repositorio en GitHub Pages, Netlify, Vercel o AWS Amplify.

> Antes de publicar, convierte `assets/img/og-image.svg` a PNG de 1200 × 630 y usa una URL absoluta en `og:image`: WhatsApp y Facebook no muestran vistas previas en SVG.
