# Guía para agentes de IA

Este sitio es una plantilla de Ixbal: HTML, CSS y JavaScript sin dependencias ni paso de compilación. Lo que ves en `index.html` es exactamente lo que se publica.

## Reglas

- **No agregues frameworks, bundlers ni dependencias de npm.** El entorno no ejecuta `npm install`.
- **Valida siempre** con `npm run check` después de cada cambio. Debe terminar en `✓`.
- **Colores, tipografía y espacios solo en `assets/css/tokens.css`.** Los títulos usan `--font-display` y el texto `--font-sans`. No escribas colores hexadecimales fuera de ese archivo; usa `var(--color-…)`.
- **Un archivo CSS por componente o sección.** Si creas uno nuevo, impórtalo en `assets/css/main.css` dentro de su capa (`components` o `sections`).
- **Clases con convención BEM:** `bloque__elemento--modificador` (por ejemplo `service-card__title`, `button--primary`).
- **JavaScript en módulos** dentro de `assets/js/modules/`, registrados en `assets/js/main.js`. Los módulos localizan elementos con atributos `data-*` y no fallan si no los encuentran.
- **Íconos** en el sprite `assets/img/icons.svg`; agrega un `<symbol id="…">` y úsalo con `<use href="assets/img/icons.svg#…">`.
- **WhatsApp, teléfono y correo de contacto los administra Ixbal** ("Tu negocio"). No los escribas ni los cambies en el HTML: si la persona pide cambiarlos, usa la herramienta `actualizar_datos_negocio`. Cada liga lleva `data-contact="whatsapp"`, `"phone"` o `"email"`; ponlo también en las ligas nuevas.

## Datos que se repiten

Cuando cambies uno de estos datos, cámbialo en **todos** sus lugares:

| Dato | Dónde aparece |
|---|---|
| Nombre del agente | `<title>`, `og:title`, `.brand__name`, `aria-label` del logo, JSON-LD, `.hero__lead`, `data-message` de los dos formularios ("Hola Rodrigo"), botón "Llamar a Rodrigo" de `#siniestros`, `#sobre-mi`, pie de página, `alt` de imágenes y `og-image.svg` |
| Cédula CNSF | `.hero__license`, la lista `.credentials` de `#sobre-mi` y el pie de página |
| Número de aseguradoras | `.hero__title`, `.hero__facts`, `.quote__steps` de `#cotizar` y `.insurers__title`; debe coincidir con la lista `.insurers__list` |
| WhatsApp | Todos los enlaces con `data-contact="whatsapp"` (formato `https://wa.me/52XXXXXXXXXX`). Los formularios toman el número del primero |
| Teléfono | Enlaces con `data-contact="phone"` (formato `tel:+52XXXXXXXXXX`: hero, `#siniestros` y `#contacto`) y `telephone` del JSON-LD |
| Correo | Enlace con `data-contact="email"` en `#contacto` y `email` del JSON-LD |
| Dirección y zonas | `#contacto`, `src` del mapa, `address` y `areaServed` del JSON-LD, "Atiendo en" de `#sobre-mi` y las opciones de `municipio` en `#cotizar` |
| Horario | Tabla `[data-hours]` en `#contacto` (`data-days`, `data-open`, `data-close`) y `openingHoursSpecification` del JSON-LD |
| Tipos de seguro | Tarjetas `.product` de `#seguros`, accesos `.pick` del hero, opciones `tipo` del paso 1 de `#cotizar` con su grupo `data-wizard-if` del paso 2 y su línea en `data-message`, pestañas de `#siniestros` y opciones `poliza` de `#renovacion` |
| Color principal | `--color-primary` en `tokens.css`, `theme-color`, `logo.svg`, `favicon.svg`, `og-image.svg`, `placeholder.svg` |

`npm run check` detecta WhatsApp o teléfonos distintos entre sí, archivos que no existen, anclas rotas, imágenes sin `alt` y JSON-LD inválido.

## El cotizador

`#cotizar` es un `<form data-wizard data-whatsapp-form>` de tres pasos (ver `assets/js/modules/wizard.js`): 1) tipo de seguro, 2) preguntas de ese tipo, 3) nombre y contacto.

- **Paso 2 condicional.** Cada grupo `.quote__group` lleva `data-wizard-if="tipo=<valor>"`, donde `<valor>` es el `value` exacto de su opción en el paso 1. Solo se ve el grupo que coincide; los campos de los demás se desactivan y no se validan.
- **Mensaje.** Sale de `data-message`, una línea por tipo de seguro (`&#10;` separa líneas). Cada `{campo}` es el `name` de un campo, y una línea cuyos campos quedaron vacíos no se envía, así que solo llega la del tipo elegido. Si agregas un campo, inclúyelo en la línea de su tipo.
- **Agregar un tipo de seguro** (por ejemplo, negocio o viaje): una opción `tipo` en el paso 1, un `.quote__group` con `data-wizard-if="tipo=<valor>"` en el paso 2 con nombres de campo propios (`negocio_giro`…), una línea `Negocio: {negocio_giro}…` en `data-message`, su tarjeta `.product` y su acceso `.pick`.
- El formulario **no cotiza ni guarda nada**: abre WhatsApp con el mensaje armado. No prometas precios en el sitio; la tabla de `#comparativa` es un ejemplo y debe seguir diciéndolo.

## Siniestros, aseguradoras y avisos

- `#siniestros` usa `assets/js/modules/tabs.js`: un botón `[data-tab="id"]` y un panel `[data-tab-panel="id"]` por tipo. Mantén la indicación de llamar primero a la cabina de la aseguradora y la de llamar al 911 si hay heridos.
- `.insurers__list` lleva los **nombres** de las aseguradoras en texto. No pongas sus logotipos sin permiso de cada marca.
- La cédula de agente de la CNSF y la nota del pie (coberturas según cada aseguradora y aviso de privacidad) son obligatorias para un agente: no las quites, cámbialas por las reales.

## Espacios de imagen

Cada foto del sitio vive en un espacio declarado en `template.json` → `imageSlots`:

```html
<figure class="media media--square" data-slot="galeria-1" data-placeholder data-hint="Foto del local · 1:1">
  <img src="assets/img/placeholder.svg" alt="Descripción de la foto" width="1200" height="1200" loading="lazy">
</figure>
```

Para poner una foto real en un espacio:

1. Cambia el `src` del `<img>` por la ruta de la foto. Las fotos que se suben desde Ixbal llegan a `images/` (por ejemplo `images/fachada.jpg`); usa esa ruta tal cual, sin mover ni renombrar el archivo.
2. Escribe un `alt` que describa la foto real y actualiza `width` y `height` con sus medidas.
3. Borra `data-placeholder` y `data-hint` del `<figure>`. Así desaparece la etiqueta de relleno.
4. No cambies la clase `media--…` ni el `data-slot`: CSS recorta la foto a la proporción del espacio.

Las fotos de `images/` que trae la plantilla son de ejemplo (generadas con IA para Rodrigo Garza): reemplázalas por las del negocio real siguiendo los mismos pasos, y borra del repositorio las de ejemplo que ya no se usen.

Si la persona sube varias fotos sin decir dónde van, asígnalas según la `label` de cada espacio en `imageSlots`. `npm run check` dice cuántos espacios siguen con imagen de relleno.

## Estructura

```
index.html                 Página única, dividida en secciones con comentarios ============
assets/css/main.css        Orden de capas e imports
assets/css/tokens.css      Identidad visual (edita aquí primero)
assets/css/base.css        Reset y elementos HTML
assets/css/layout.css      Contenedores, secciones, rejillas
assets/css/components/     Piezas reutilizables (botón, tarjeta, encabezado…)
assets/css/sections/       Estilos propios de cada sección de la página
assets/css/utilities.css   Clases de una sola responsabilidad
assets/js/main.js          Registra los módulos
assets/js/modules/         Comportamiento (menú, horario y "Te contesto ahora", año, pestañas de siniestros, cotizador por pasos, formularios a WhatsApp)
assets/img/                Logo, íconos e imágenes de relleno
images/                    Fotos que sube la persona desde Ixbal (se crea al subir la primera)
template.json              Metadatos para la galería de plantillas de Ixbal
scripts/check.mjs          Validador sin dependencias
```

## Tareas comunes

- **Solo algunos seguros:** borra la tarjeta `.product`, el `.pick` del hero, la opción `tipo`, su `.quote__group`, su línea de `data-message`, la pestaña y el panel de `#siniestros`, y la opción de `#renovacion`. Quita su espacio de imagen de `imageSlots`.
- **Otra ciudad:** cambia la ciudad en textos, títulos y JSON-LD, las opciones de `municipio` del cotizador, "Atiendo en" de `#sobre-mi` y el mapa.
- **Aseguradoras:** edita `.insurers__list` y el número en los lugares de la tabla de arriba.
- **Comparativa de otro producto:** cambia el `<caption>`, los encabezados y las filas de `.compare-table`; la columna recomendada lleva `.compare-table__pick`. Conserva la nota de que los precios son de ejemplo.
- **Sin oficina:** borra la dirección de `#contacto`, el mapa (`.contact__map`) y `address` del JSON-LD, y quita la mención a la oficina en `.contact__lead`.
- **Quitar una sección:** borra el `<section>` completo y su enlace en `.site-nav__list`.
- **Nuevo espacio de imagen:** agrega el `<figure class="media" data-slot="…">` y su entrada en `imageSlots` de `template.json`.
