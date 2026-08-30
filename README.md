# Alquiler Gunitadora con Técnico — sitio web

Rediseño del sitio del cliente (alquilergunitadora.com), en HTML/CSS/JS plano, pensado para hospedar en Netlify.

## Estructura

- `index.html` — Inicio
- `quienes-somos.html` — Quiénes somos
- `servicios.html` — Productos y servicios
- `galeria.html` — Galería (con placeholders)
- `contacto.html` — Contacto (formulario Netlify Forms)
- `gracias.html` — Página de agradecimiento tras enviar el formulario
- `styles.css`, `script.js` — estilos y JS compartidos (nav móvil, animaciones ligeras)

## Marca

- **Logo**: `images/logo-fondotrans.png` (original) → recortado a `images/logo-tight.png` (sin margen transparente, usado en el header) y a favicons `images/favicon-{16,32,180,512}.png`.
- **Colores**: Azul Industrial `#004080`, Naranja Maquinaria `#E67300`, Gris Neutro `#A0A0A0` — definidos como variables CSS en `styles.css` (`--navy`, `--accent`, `--grey`, etc.).
- **Tipografía**: "Big Shoulders" (slab serif angular, títulos) + "Inter" (texto), vía Google Fonts.
- **Foto de flota**: `images/maquinas2.jpeg` tenía una franja negra (artefacto de panorámica) en la esquina superior — se recortó a `images/flota-maquinas.jpg` y se usa como banner en `index.html`. El resto de imágenes originales de la carpeta (`alain.jpeg`, `gunite.jpeg`, `maquinas.jpeg`, variantes de logo) no se usan en el diseño actual.

## Pendiente antes de publicar

1. **Más fotos reales para la galería**: `galeria.html` sigue con 6 placeholders (`.ph`) — sustituir por fotos de obras cuando el cliente las pase.
2. **Contenido de servicios**: la lista actual en `servicios.html` es una propuesta razonable a partir del sector; confirmar con el cliente si hay servicios/precios concretos a añadir o quitar.
3. **Dirección física**: no se encontró en el sitio actual (probablemente solo en un mapa embebido) — pedir al cliente y añadirla en `contacto.html` y el footer.
4. **Redes sociales**: si el cliente tiene, añadir enlaces en el footer.

## Formulario de contacto (Netlify Forms)

El formulario en `contacto.html` usa [Netlify Forms](https://docs.netlify.com/manage/forms/setup/): no necesita backend, Netlify detecta el `<form data-netlify="true">` en el despliegue.

Para que las respuestas lleguen por email al cliente:
1. Panel de Netlify → tu site → **Forms** → **Settings and usage** → **Add notification** → **Email notification**.
2. Introduce el email del cliente y guarda.

Incluye un campo honeypot (`bot-field`) oculto para reducir spam.

## Despliegue

```bash
npm install -g netlify-cli
netlify login
netlify init      # o "netlify deploy --prod" tras vincular el site
```

O bien, sin CLI: en el dashboard de Netlify → **Add new site → Import an existing project** → conectar el repo de GitHub → deploy automático en cada push a `main`.
