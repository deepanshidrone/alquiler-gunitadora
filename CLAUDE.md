# Alquiler Gunitadora con Técnico — contexto del proyecto

Sitio web para un cliente de alquiler de gunitadoras (máquinas de proyección de hormigón/gunita) con técnico incluido, en Girona/Catalunya. Sustituye a la web antigua (IONOS "Mi Web" / Website Builder) en el dominio real **alquilergunitadora.com**.

## Stack

HTML/CSS/JS plano, sin build ni framework. Pensado para desplegar en Netlify (gratis en el plan actual, sin necesidad de build step).

- `index.html`, `quienes-somos.html`, `servicios.html`, `galeria.html`, `contacto.html`, `gracias.html`
- `styles.css` — todos los estilos (variables CSS en `:root`)
- `script.js` — nav móvil (hamburguesa) + animaciones `reveal` con IntersectionObserver
- `netlify.toml` — `publish = "."`, headers de seguridad básicos

## Marca

- **Colores** (variables en `styles.css`): `--navy: #004080` (Azul Industrial), `--accent: #E67300` (Naranja Maquinaria), `--grey: #A0A0A0` (Gris Neutro). Alias internos `--dark`/`--dark-2` apuntan a navy/navy-light por compatibilidad con estilos antiguos.
- **Tipografía**: Zilla Slab (títulos, slab serif) + Inter (cuerpo), vía Google Fonts. *(Antes se usó Big Shoulders; se cambió a Zilla Slab tras comparar varias opciones en una demo con fuentes embebidas en base64 — ver historial de commits.)*
- **Logo actual**: `images/logo5-tight.png` (recortado a su bounding box real, sin margen transparente) — usado en el header de las 6 páginas junto al texto `alquilergunitadora.com` (`.brand__name`, fuente Zilla Slab). Favicons generados a partir del mismo logo: `images/favicon-{16,32,180,512}.png`.
- **Otros logos en `images/`** (`logo.jpeg`, `logo2/3/4`, `logo-fondotrans.png`, `logo-nuevo*`, etc.) son versiones anteriores/descartadas — no están en uso, se conservan como histórico.
- **Foto de flota real**: `images/flota-maquinas.jpg` (recortada de `maquinas2.jpeg` para quitar un artefacto de panorámica). Se usa como fondo del hero en `index.html` (texto centrado encima, overlay navy) y como banner en `quienes-somos.html`.
- El resto de imágenes en `images/` (`alain.jpeg`, `gunite.jpeg`, `maquinas.jpeg`) no se usan en el diseño actual.

## Diseño del hero (home)

Se probaron dos direcciones (mockups con fuentes/imágenes embebidas en un Artifact) y el cliente eligió la **Opción A**: foto de la flota a pantalla completa como fondo del hero, con overlay navy y todo el texto centrado (antes estaba alineado a la izquierda sobre fondo sólido). El banner de flota independiente que había en `index.html` se quitó (redundante) y se trasladó a `quienes-somos.html`.

## Formulario de contacto

`contacto.html` usa **Netlify Forms** (`data-netlify="true"`, honeypot `bot-field`). Redirige a `gracias.html` tras el envío.

- El site tenía el flag `processing_settings.ignore_html_forms: true` que impedía la detección del formulario — se corrigió (ver `netlify api updateSite`).
- Notificación por email configurada vía Netlify (hook tipo `email`, evento `submission_created`) → llega a `info@alquilergunitadora.com`. Probado end-to-end y funciona.

## Despliegue

- **Repo**: [github.com/deepanshidrone/alquiler-gunitadora](https://github.com/deepanshidrone/alquiler-gunitadora) (público), cuenta `deepanshidrone`.
- **Netlify**: site `alquiler-gunitadora` (cuenta `ddslloret` / team "NexeAI", plan Free). Deploys manuales vía `netlify deploy --prod --dir=.` (no hay CI/build automático conectado a git todavía — pendiente si se quiere deploy automático en cada push).
- **Dominio de producción**: `alquilergunitadora.com` (dominio y DNS siguen gestionados en **IONOS**, no se migró el registrador, solo los registros DNS de la web).
  - Registro `A` en `@` → `75.2.60.5` (Netlify)
  - Registro `CNAME` en `www` → `alquiler-gunitadora.netlify.app`
  - **MX, SPF, DKIM, DMARC intactos en IONOS** — el correo NO se migró, sigue en IONOS.
  - SSL emitido automáticamente por Netlify (Let's Encrypt).

## IONOS (cliente)

- El cliente tenía **"IONOS Mi Web - Pack Premium"** (~34€/mes) para hosting/website builder — ya no se usa (la web vive en Netlify).
- Se hizo **downgrade a "IONOS Correo Básico 1"** (1,50€/mes) para conservar solo el dominio + 1 buzón de correo (`info@alquilergunitadora.com`), que sigue en uso real.
- Antes del downgrade se hizo una **copia de seguridad de la web antigua** (HTML renderizado + imágenes reales) en `~/Desktop/backup-web-vieja-alquilergunitadora/` (fuera del repo), accediendo directamente a la URL interna de IONOS `s548654284.web-inicial.es` (el hosting antiguo seguía vivo ahí aunque el dominio ya apuntaba a Netlify).
- El cliente tiene **otros 2 dominios** también hosteados en IONOS — pendientes de revisar en otra sesión, no tocados en este proyecto.

## Contenido pendiente / a confirmar con el cliente

- La lista de servicios en `servicios.html` es una propuesta razonable del sector, no confirmada línea a línea con el cliente.
- Dirección física del negocio: no se encontró en la web antigua, pendiente de pedir si se quiere añadir.
- `galeria.html` sigue con placeholders (`.ph`) — faltan fotos reales de obras del cliente.
- Redes sociales: añadir si el cliente las tiene.

## Notas técnicas / gotchas aprendidos

- El breakpoint de menú hamburguesa está en `1140px` (no el más habitual 768/860px) porque el header (logo + texto de marca + nav + CTA) no cabía en el contenedor de 1140px de ancho hasta ese punto — ver `.site-header .nav.container { max-width: 1200px }` y los ajustes de gap/tamaño en `styles.css`.
- `.brand__name` tiene fallback de `overflow:hidden; text-overflow:ellipsis` en móvil para pantallas muy estrechas (~320px).
- Cuidado al hacer cambios de DNS en IONOS: el panel avisa qué registros se desactivan antes de confirmar — siempre revisar que no aparezcan `MX`/`SPF`/`_domainkey`/`_dmarc` en esa lista antes de confirmar cualquier cambio de pack o de registro.
