# Plantilla NoExpertise · Bodas y eventos

Web romántica para wedding planners, fincas y espacios, fotógrafos de bodas y catering: portada con foto,
servicios, galería en mosaico, packs con precio opcional, proceso, opiniones en carrusel y formulario con fecha,
tipo de evento e invitados.

- Sectores: bodas, eventos, wedding-planner, fotografia-bodas, catering, espacios. Estilo: romantico. Plan mínimo: presencia.
- Secciones: hero, servicios, galeria, packs, proceso, opiniones, preguntas, contacto (+ `texto` genérico).
- Campos propios: `packs.packs[]` {nombre, precio, texto, incluye, destacado}, `packs.nota`, `testimonios[].nota`,
  `contacto.tipos_evento`, `contacto.boton`.
- Diseño: marfil + salvia + dorado, todo centrado, serif cursiva para títulos, versalitas espaciadas, filetes
  dorados, cabecera con el nombre en el centro y el menú repartido a los lados, pie centrado con redes.
- SEO de base incluido (`src/lib/seo.ts`): canónica, og:image, JSON-LD del negocio y de las preguntas frecuentes, `robots.txt`, `sitemap.xml` y `llms.txt`; `noindex` si `seo.indexable: false`.
- Varias páginas (opcional, `paginas:` en la raíz de `site.yaml`): Inicio + una página por entrada (`{ ruta, titulo, secciones: "a; b" }`, hasta 6) con el menú hecho solo; reparto recomendado en `template.yaml → paginas`. Sin la clave, una sola página larga.
- Contenido editable en `site.yaml`; metadatos en `template.yaml`; reglas para el agente en `CLAUDE.md`;
  fotos a generar en `fotos.json`.

```
npm ci
npm run dev      # desarrollo
npm run build    # web estática en dist/
```

Basada en [AstroWind](https://github.com/onwidget/astrowind) © onWidget, licencia MIT (`LICENSE-ASTROWIND.md`).
Imágenes de relleno (`public/placeholders/`) dibujadas para esta plantilla, sin licencias de terceros.

## Páginas propias
- Cada elemento de `secciones.servicios.items` con `detalle` tiene su página en `/servicios/<slug>/` (`src/pages/[coleccion]/[slug].astro`).
- Campos opcionales de los elementos con ficha: `detalle` (párrafos separados por una línea en blanco), `slug` e `imagenes` («/fotos/a.jpg; /fotos/b.jpg»).
