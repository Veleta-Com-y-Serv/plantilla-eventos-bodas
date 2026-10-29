# Plantilla «Bodas y eventos» (NoExpertise)

Web estática Astro 5 + Tailwind 4, derivada de AstroWind (MIT, ver `LICENSE-ASTROWIND.md`). Estilo: **romántico**
(marfil + salvia + dorado, todo centrado, títulos en serif cursiva, antetítulos en versalitas espaciadas, filetes
dorados como ornamento, cabecera con el nombre centrado y el menú repartido a los dos lados, pie centrado con redes).
Todo en castellano de España, frases cortas, sin jerga.

## Dónde está cada cosa
- **Textos, contacto, menú, SEO y secciones**: `site.yaml` (raíz). Es la única fuente de contenido. `src/data/site.ts` lo carga y lo tipa; no pongas textos fijos en los componentes.
- **Colores**: `site.yaml → colores` (primario = salvia de botones y precios, acento = dorado de filetes y ornamentos, fondo = marfil, texto). `src/layouts/Base.astro` los convierte en variables CSS (`--c-primario`…) y Tailwind los expone como `bg-primario`, `text-acento`, `bg-primario/10`… Los derivados (salvia claro, dorado para texto, filetes) se calculan con `color-mix` en `src/styles/global.css`.
- **Tipografías y aspecto general**: `src/styles/global.css` (`--fuente-texto`, `--fuente-titulos`, `--fuente-detalle`, `.ornamento`, botones `.btn`/`.btn-sec`, cabecera `.cabecera--bodas`, pie `.pie--bodas`). Fuentes del sistema: no añadas fuentes externas.
- **Imágenes**: en `public/`. Las fotos van en `public/fotos/`; las de respaldo en `public/placeholders/`. En `site.yaml` se referencian con ruta absoluta (`/fotos/hero.jpg`). Fotos sin caras reconocibles: detalles, mesas, espacios, parejas de espaldas o lejos.
- **Logo**: `site.yaml → logo` (ruta en `public/`); vacío = nombre en cursiva, centrado.
- **Menú**: la primera mitad de `site.yaml → menu` va a la izquierda del nombre y el resto a la derecha (desde 1100 px; por debajo, menú plegable). Con 4-6 entradas cortas queda equilibrado.
- **Pie «Web creada con NoExpertise»**: `site.yaml → marca_pie`.

## Secciones
Cada sección es un componente que solo se pinta si `site.yaml → secciones.<id>.activa` es `true`. Se pintan en el orden en que aparecen en `site.yaml`. Un apartado nuevo puede reutilizar un diseño con `tipo: <id>`.
- `hero` → `hero.astro`: foto a todo el ancho (`imagen`) con caja marfil centrada: `antetitulo`, `titulo`, `texto`, `cta`, `cta_secundario` (opcional).
- `servicios` → `servicios.astro`: `items` con `imagen` en tarjetas altas 3:4 (sin imagen, `icono` en círculo dorado), `cta` opcional.
- `galeria` → `galeria.astro`: `galeria` (rutas) en mosaico de columnas con alturas distintas, `cta` opcional. Sin fotos no se pinta.
- `packs` → `packs.astro`: `packs`, `cta` común, `nota`. Sin packs no se pinta.
- `proceso` → `proceso.astro`: `items` numerados en vertical, centrados, unidos por un filete dorado.
- `opiniones` → `opiniones.astro`: `testimonios` en carrusel horizontal sin JavaScript (scroll-snap). Sin testimonios no se pinta.
- `preguntas` → `preguntas.astro`: `preguntas` en acordeón. Sin preguntas no se pinta.
- `contacto` → `contacto.astro`: datos de `contacto`, `texto`, `imagen`, `tipos_evento`, `boton`; formulario con fecha, tipo e invitados.
- `texto` → `texto.astro`: apartado genérico centrado (`tipo: texto`) con `imagen`, `items`, `cta`.

## Campos propios de esta plantilla (además del esquema común)
- `packs.packs`: `[{nombre, precio, texto, incluye, destacado}]`. `precio` siempre como cadena («desde 2.400 €»); vacío = no se muestra. `incluye` es UNA cadena separada por «;» («Ensayo; Coordinación; Flores»), nunca una lista. `destacado: true` en uno como mucho. Máximo 24.
- `packs.nota`: aviso pequeño bajo los packs (IVA, precios orientativos…).
- `testimonios[].nota`: detalle bajo los nombres («Boda en Pedraza, mayo de 2024»).
- `contacto.tipos_evento`: cadena separada por «;» con las opciones del desplegable (vacía = Boda; Comunión; Empresa; Otro). `contacto.boton`: texto del botón del formulario.
- `boton_cabecera` vacío a propósito: si se rellena, cierra el lado derecho del menú como enlace con filete dorado.

## Reglas para órdenes
1. **Activar o quitar una sección**: cambia `activa: true|false` en `site.yaml`. No borres componentes.
2. **Reordenar secciones**: mueve el bloque dentro de `secciones:` en `site.yaml`.
3. **Cambiar textos, teléfonos, horarios, precios, fotos**: solo en `site.yaml`. Si un texto contiene comas dentro de `{ … }`, ponlo entre comillas. Precios siempre entre comillas.
4. **Listas planas**: nunca una lista dentro de un elemento de lista (ni siquiera en `incluye`).
5. **No inventes** precios, opiniones ni datos: si el cliente no los da, deja `precio: ""`, `incluye: ""` o `testimonios: []`; el componente lo oculta.
6. **Página nueva**: crea `src/pages/<slug>.astro` usando `import Base from '../layouts/Base.astro'` y `<Base titulo="…" descripcion="…">…</Base>`; enlázala en `site.yaml → menu`. Ejemplo: `src/pages/aviso-legal.astro`.
7. **Sección nueva**: mejor un id nuevo con `tipo:` (`texto`, `servicios`, `packs`…). Si hace falta un diseño nuevo, crea `src/components/secciones/<id>.astro` con `seccion(Astro.props.id)`, `Seccion` y `Titular` (centrado, con ornamento). `src/pages/index.astro` la recoge sola.
8. **No tocar** `src/pages/index.astro`, `src/layouts/Base.astro`, `tailwind.config.mjs` ni `src/styles/comun.css` salvo orden explícita de diseño.
9. Mantén: `lang="es-ES"`, imágenes con `Imagen.astro` (carga diferida), sin JavaScript nuevo salvo necesidad real, 16 px de margen lateral (`.contenedor`), sin scroll horizontal en móvil (el carrusel se desplaza solo dentro de sí mismo), ids `hero` y `contacto`.
10. El formulario no tiene servidor: abre el correo (`mailto`) con el email de `site.yaml`. Para envío real, cambia `action` en `src/components/ui/FormularioContacto.astro`.

## Construir
```
npm ci && npm run build      # resultado en dist/
npx astro preview            # ver dist/ en http://localhost:4321
```
Antes de dar una orden por buena: `npm run build` sin errores.

Fotos: `fotos.json` describe las fotos de `public/fotos/` (se generan aparte, sin caras reconocibles); los SVG de `public/placeholders/` quedan de respaldo.
