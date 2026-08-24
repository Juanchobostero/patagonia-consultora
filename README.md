# Patagonia Consultora

Landing page institucional de **Patagonia Consultora**, estudio de soluciones
integrales (gestión documental, contable, laboral y legal) para empresas
proveedoras y contratistas de Vaca Muerta. Neuquén, Argentina.

🔗 Producción: [patagoniaconsultora.com.ar](https://patagoniaconsultora.com.ar)

## Stack

- [Astro 7](https://astro.build) — sitio 100% estático, sin framework de UI.
- TypeScript + [`@astrojs/check`](https://www.npmjs.com/package/@astrojs/check) para el chequeo de tipos.
- [`@astrojs/sitemap`](https://www.npmjs.com/package/@astrojs/sitemap) para el `sitemap-index.xml` de SEO.
- Gestor de paquetes: **pnpm**.

## Estructura del proyecto

```text
src/
├── consts.ts          # Datos centrales: SEO, contacto, redes sociales, Google Form, equipo
├── layouts/            # <head> con SEO, Open Graph y JSON-LD
├── components/         # Una sección de la landing por componente (Hero, Servicios, Equipo, ...)
├── styles/             # CSS del sitio (paleta navy/gold)
└── pages/
    └── index.astro     # Ensambla la página a partir de los componentes

public/                 # Estáticos: logo, favicons, imágenes
```

Ver [DOC.md](./DOC.md) para el detalle completo de decisiones de diseño, SEO
y contenido pendiente de la clienta.

## Desarrollo local

Requiere Node `>=22.12.0` y pnpm.

| Comando         | Acción                                       |
| :--------------- | :-------------------------------------------- |
| `pnpm install`    | Instala las dependencias                      |
| `pnpm dev`        | Levanta el servidor local en `localhost:4321` |
| `pnpm check`      | Chequea tipos de Astro/TypeScript             |
| `pnpm build`      | Genera el sitio de producción en `./dist/`    |
| `pnpm preview`    | Previsualiza el build antes de deployar       |

## Deploy

Sitio estático — Vercel detecta el preset **Astro** automáticamente (build
`pnpm build`, output `dist/`), sin necesidad de `vercel.json`. Ver
[DOC.md, sección 6](./DOC.md#6-deploy-en-vercel) para el paso a paso.
