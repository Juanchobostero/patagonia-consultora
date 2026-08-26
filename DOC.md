# Patagonia Consultora — Documentación del proyecto

Landing page construida en [Astro](https://astro.build) a partir del mockup
`PATAGONIA-SOLUCIONES-MOCKUP.html` provisto por la clienta, con los ajustes
indicados en las capturas de anotaciones. Gestor de paquetes: **pnpm**.

---

## 1. Stack y estructura

- **Astro 7** (sitio 100% estático, sin framework de UI — HTML/CSS puro, igual que el mockup).
- **@astrojs/sitemap** para generar `sitemap-index.xml` automáticamente.
- **TypeScript** (`astro check` disponible vía `pnpm check`).

```
src/
├── consts.ts              # Datos centrales: SEO, email, Google Form URL, redes sociales
├── layouts/
│   └── Layout.astro       # <head> con SEO, Open Graph, Twitter Card, JSON-LD, fuentes
├── styles/
│   └── global.css         # CSS del mockup, adaptado (paleta navy/gold, tipografías)
├── components/
│   ├── Nav.astro
│   ├── Hero.astro
│   ├── Challenge.astro    # "El desafío"
│   ├── Solutions.astro    # "Soluciones" (grid de 4 áreas)
│   ├── Services.astro     # "Nuestros servicios" (detalle, 4 tarjetas)
│   ├── HowWeWork.astro    # "Cómo trabajamos"
│   ├── Team.astro         # "Quiénes somos" (2 integrantes)
│   ├── CtaFinal.astro
│   ├── Footer.astro
│   ├── WhatsappFloat.astro
│   └── BackToTop.astro    # Flecha "volver arriba", apilada sobre el botón de WhatsApp
└── pages/
    └── index.astro         # Ensambla todos los componentes dentro de Layout

public/
├── logo.png                 # Logo completo extraído del mockup (isotipo + wordmark)
├── og-image.jpg              # Imagen para vista previa en redes (og:image / twitter:image, 1200×630)
├── favicon.ico                # Favicon multi-resolución (16/32/48px), fallback para rastreadores
├── favicon-16x16.png         # Favicon (recorte del isotipo "PC" del logo real)
├── favicon-32x32.png
├── apple-touch-icon.png      # Ícono para iOS/Safari (180×180)
├── icon-512.png              # Versión grande del isotipo (uso futuro: PWA/manifest)
├── images/
│   ├── hero-vaca-muerta.webp # Foto de portada del hero, recorte del collage (ver sección 2)
│   ├── patagonia-image.webp  # Collage original de 4 fotos provisto por la clienta (sin usar directo)
│   ├── cristina-image.webp   # Foto de equipo: Dra. Cristina Rodríguez
│   └── patricia-image.webp   # Foto de equipo: Cra. Patricia Huenohueque
└── robots.txt
```

Cada sección del mockup quedó como un componente Astro independiente para
poder editar contenido sin tocar el resto del sitio.

---

## 2. Cambios aplicados según las capturas con anotaciones en rojo

| # | Captura | Cambio aplicado |
|---|---------|------------------|
| 1 | Botón "Conversemos" (nav) — "¿se podrá abrir el form de Google para enviar mail?" | El botón "Conversemos" del nav, el botón "Conversemos" del CTA final y "Solicitar una reunión" del hero ahora abren el **Google Form** real (en pestaña nueva). Ver sección 4. |
| 2 | Tercera tarjeta del equipo (Abogada — Derecho Empresarial y Contratos) marcada "Eliminar" | Se eliminó esa tarjeta. `Team.astro` ahora muestra solo 2 integrantes (Abogada — Derecho Laboral y Empresarial / Cra. Patricia Huenohueque). El grid pasó de 3 a 2 columnas. |
| 3 | Tarjeta "Gestión Contable y Administrativa" en la grilla de Soluciones marcada "Eliminar" | Se eliminó esa tarjeta de `Solutions.astro`. La grilla pasó de 5 a 4 columnas. |
| 4 | Tarjeta "Administración de Personal y Nómina" en Servicios — "Agregar items" | Se agregaron al checklist: *"Liquidación de Cargas Sociales e Impuestos a las Ganancias de 4ta Categoría"* y *"Presentación de Libro de Sueldo Digital"*. |
| 5 | Tarjeta de servicio "Gestión Contable y Administrativa" — "Esta sección se elimina" | Se eliminó por completo esa tarjeta de `Services.astro` (quedan 4: Gestión Documental, Administración de Personal y Nómina, Auditoría y Control Interno, Asesoría Legal). |
| 6 | Párrafo del hero — "Empresas" reemplazando la palabra tachada | El texto quedó: *"Acompañamos a **Empresas** en la organización documental, administrativa, contable, laboral y legal necesaria..."* (antes decía "PyMEs"). |

Nota de consistencia: como se quitó "Gestión Contable y Administrativa" de
Soluciones y de Servicios, también se removió la palabra "Contable" de la
línea de tags del footer (`Footer.astro`).

### Foto real del hero

El fondo del hero es una foto propia de la empresa (equipo en un yacimiento
de Vaca Muerta al atardecer), provista por la clienta como
`public/images/patagonia-image.webp` — un collage de 4 fotos en formato
retrato (900×1600). A pedido de la clienta se recortó únicamente el panel
superior (los dos profesionales con la campera de la marca y el equipo de
perforación de fondo) como `public/images/hero-vaca-muerta.webp` (900×630),
usado en `Hero.astro`. El archivo original (`patagonia-image.webp`) se dejó
en `public/images/` por si más adelante quieren usar otro de los paneles
del collage. No requiere atribución de terceros al ser una foto propia, por
lo que no lleva crédito.

Todas las capturas se pudieron leer con claridad — no hubo ninguna anotación dudosa.

## 2.1 Correcciones — `Correciones.pdf` (2026-08-26)

La clienta envió un PDF con textos resaltados/tachados a mano para una
segunda ronda de ajustes. Se implementaron los 7 puntos:

| # | Pedido | Cambio aplicado |
|---|--------|------------------|
| 1 | Reemplazar el párrafo de "El desafío" | Nuevo texto en `Challenge.astro` (los dos `<p>` de `.challenge-right`). |
| 2 | Reemplazar la cita destacada de "El desafío" | Nuevo texto en `.challenge-quote` (`Challenge.astro`). |
| 3 | Reemplazar título/copy del CTA final | `CtaFinal.astro`: nuevo `<h2>` ("¿Tu empresa está preparada para las exigencias de Vaca Muerta?"), nueva bajada ("Conversemos sobre sus objetivos."), nuevo párrafo, y se agregó la firma "PATAGONIA CONSULTORA · Gestión · Control · Cumplimiento" al pie del bloque (`.cta-signature` en `global.css`). |
| 4 | En las tarjetas del equipo, dejar solo la profesión (sin especialidad) | `TEAM` en `consts.ts`: Cristina Rodríguez → *"Abogada"*; Patricia Huenohueque → *"Contadora Pública Nacional"*. |
| 5 | Quitar el tag "PyMEs proveedoras y contratistas · Vaca Muerta" del hero | Se eliminó el `<div class="eyebrow">` de `Hero.astro` y la clase `.eyebrow` (sin uso) de `global.css`. |
| 6 | Agregar ícono + link de Instagram | Se agregó `SOCIAL.instagram` en `consts.ts` (`instagram.com/patagoniaconsultoranqn`) y el ícono correspondiente en `Footer.astro` / `.social-instagram` en `global.css`, con el mismo patrón que WhatsApp/LinkedIn. |
| 7 | Cambiar la foto de portada (hero) | Ver "Foto real del hero" arriba — ahora usa `public/images/hero-vaca-muerta.webp`. |

### Número de WhatsApp (fuera del PDF, ida y vuelta por chat)

El número original provisto (`299578813`, 9 dígitos) nunca llegó a
funcionar en WhatsApp, ni con el `9` (`wa.me/549299578813`) ni sin él
(`wa.me/54299578813`) — el problema no era el formato del link, sino que
faltaba un dígito: el número real de WhatsApp es **2995788713** (10
dígitos). Quedó configurado como:

- `SOCIAL.whatsapp` (`consts.ts`): `https://wa.me/5492995788713`
- `SITE.phoneDisplay` (`consts.ts`): `+54 9 2995 78-8713`
- Link `tel:` del footer (`Footer.astro`): `tel:+5492995788713`

### Navegación por ancla sin `#hash` en la URL

Se pidió que la URL se mantenga siempre limpia (`localhost:4321/`, o el
dominio que sea) aunque se navegue entre secciones con el menú, los botones
o el footer. Los `<a href="#soluciones">` etc. se mantuvieron (funcionan
igual sin JavaScript), pero en `Layout.astro` se agregó un script que:

1. Intercepta el click en cualquier link interno (`a[href^="#"]`).
2. Hace `preventDefault()` — así el navegador **nunca** agrega el `#hash` a
   la barra de direcciones.
3. Calcula el scroll a mano (`window.scrollTo({ behavior: 'smooth' })`),
   restando la altura del nav sticky para que la sección no quede tapada.

Se probó haciendo click en cada link del menú, del hero y del footer: la
URL se queda siempre igual y el scroll llega a la sección correcta.

### Botón "volver arriba"

`BackToTop.astro` agrega una flecha flotante para volver al inicio de la
página. Queda apilada justo encima del botón de WhatsApp, sin superponerse
(misma columna a la derecha, con 14px de separación entre los dos), y se
comporta igual en mobile:

- Aparece recién después de bajar ~480px de scroll (antes está oculta, no
  ocupa lugar ni tapa nada cerca del hero).
- Al hacer click hace scroll suave al inicio — es un `<button>`, no un
  link con `#hash`, así que no interfiere con la URL limpia del punto
  anterior.

Se verificó con captura de pantalla en desktop y mobile que los dos botones
no se solapan en ningún momento.

---

## 3. SEO implementado

En `src/layouts/Layout.astro` y `src/consts.ts`:

- `<title>` y `<meta name="description">` específicos del negocio.
- Etiqueta canónica (`rel=canonical`) generada dinámicamente con `Astro.site`.
- Open Graph completo (`og:title`, `og:description`, `og:image` +
  `og:image:width`/`height`/`alt`, `og:type`, `og:locale`, `og:site_name`).
- Twitter Card (`summary_large_image`).
- JSON-LD `ProfessionalService` (nombre, descripción, área de servicio,
  dirección en Neuquén, `logo`, `image`, `sameAs` con LinkedIn e Instagram,
  áreas de conocimiento).
- `robots.txt` + sitemap automático (`@astrojs/sitemap`, se genera en cada build).
- `lang="es"` en `<html>`, `theme-color` de marca.
- Imágenes con `width`/`height` explícitos para evitar layout shift.
- **Favicon con el logo real**: se recortó el isotipo "PC" del `logo.png`
  original (esquina superior izquierda del logo, sin el texto "Patagonia
  Consultora") y se generaron `favicon-16x16.png`, `favicon-32x32.png`,
  `apple-touch-icon.png` y un `favicon.ico` multi-resolución (16/32/48px),
  todos referenciados en `Layout.astro`. Así la pestaña del navegador
  muestra el logo de la marca en vez de un ícono genérico, y los
  rastreadores/clientes que piden `/favicon.ico` directo también lo
  encuentran.
- **Imagen OG dedicada** (`public/og-image.jpg`, 1200×630): compuesta a
  partir de la foto real del hero (`hero-vaca-muerta.webp`) más el logo en
  una placa blanca y el copy "Soluciones integrales para empresas en Vaca
  Muerta · Neuquén, Argentina". Reemplaza el uso anterior de `logo.png`
  (apaisado, no pensado para vista previa de redes) como `og:image` /
  `twitter:image`.

**Pendiente antes de indexar en producción (no depende del código):**
- Confirmar que el dominio `patagoniaconsultora.com.ar` quede conectado en
  Vercel — `astro.config.mjs` (`site:`) y `src/consts.ts` (`SITE.url`) ya
  están configurados con ese dominio.
- Dar de alta el sitio en Google Search Console y enviar el sitemap
  (`https://patagoniaconsultora.com.ar/sitemap-index.xml`) una vez esté el
  dominio definitivo.

---

## 4. Google Form

Los botones **"Conversemos"** (nav y CTA final) y **"Solicitar una
reunión"** (hero) abren el Google Form real de contacto, en pestaña nueva,
a través de la constante `GOOGLE_FORM_URL` en `src/consts.ts`.

---

## 5. Contenido pendiente de la clienta

### Ya incorporado (desde `datos-patagonia.txt`)

- **Equipo real**: `src/consts.ts` (array `TEAM`, usado en `Team.astro`)
  ahora tiene los nombres, títulos y LinkedIn personal de las dos
  profesionales — el nombre de cada tarjeta es un link a su perfil:
  - Dra. Cristina Rodríguez (Abogada) — [linkedin.com/in/rodriguezscristina](https://www.linkedin.com/in/rodriguezscristina)
  - Cra. Patricia Huenohueque (Contadora Pública) — [linkedin.com/in/patricia-huenohueque-070b2827b](https://www.linkedin.com/in/patricia-huenohueque-070b2827b)
- **Teléfono/WhatsApp de la consultora** (`+54 9 2995 78-8713`): ahora
  `SOCIAL.whatsapp` en `src/consts.ts` apunta a
  `https://wa.me/5492995788713` (ver sección 2.1 — el número original que
  nos habían pasado estaba incompleto) (botón flotante y footer), y se
  agregó `SITE.phoneDisplay` como link `tel:` en el footer y en el JSON-LD
  de SEO.
- **Íconos de redes en el footer**: los links de contacto del footer (antes
  texto plano) ahora son íconos circulares con el color de marca de cada
  red — ver `.footer-social` en `global.css` y `Footer.astro`.
  - **WhatsApp** (verde `#25D366`): usa el número real, `wa.me/5492995788713`.
  - **LinkedIn** (azul `#0A66C2`): apunta a la página de la empresa
    (`SOCIAL.linkedin` en `src/consts.ts`,
    `linkedin.com/company/patagonia-consultora-soluciones-integrales`).
  - **Instagram**: `SOCIAL.instagram` en `src/consts.ts`
    (`instagram.com/patagoniaconsultoranqn`) — ver sección 2.1.
- **No se usó (por ahora)**: la universidad de cada profesional (UNNE /
  Universidad Nacional de Quilmes) no tiene un lugar en el diseño actual de
  las tarjetas de equipo — quedan solo nombre + rol, igual que en el
  mockup. Avisen si quieren que se agregue.

### Ya incorporado (fotos del equipo)

- **Fotos del equipo**: `Team.astro` muestra `cristina-image.webp` y
  `patricia-image.webp` (`public/images/`) en vez de las iniciales, con
  `object-fit: cover` dentro del contenedor cuadrado responsivo.

### Todavía pendiente (no vino en `datos-patagonia.txt`)

- **Dominio definitivo**: ver sección 3.

---

## 6. Deploy en Vercel

El proyecto es un sitio estático de Astro — Vercel lo detecta
automáticamente (framework preset "Astro"), no requiere `vercel.json`.

### Opción A — desde el dashboard de Vercel
1. Subir este repo a GitHub/GitLab/Bitbucket.
2. En Vercel: **Add New → Project** → importar el repo.
3. Vercel detecta Astro solo. Build command: `pnpm build` (o `astro build`).
   Output directory: `dist`.
4. Deploy.

### Opción B — desde la CLI
```sh
pnpm add -g vercel   # si no está instalada
vercel                # deploy de preview
vercel --prod          # deploy a producción
```

### Variables de entorno
No se usan variables de entorno en build time: los datos de contacto y el
link del Google Form están en `src/consts.ts` (no son secretos). Si más
adelante se agrega analytics o un endpoint de formulario propio, ahí sí
convendría mover esos valores a variables de entorno de Vercel.

---

## 7. Comandos

```sh
pnpm install     # instalar dependencias
pnpm dev         # servidor local en http://localhost:4321
pnpm build       # build de producción en ./dist
pnpm preview     # previsualizar el build de producción
pnpm check       # chequeo de tipos de Astro/TypeScript
```

---

## 8. Verificación realizada

- `pnpm build` y `pnpm check` corren sin errores.
- Se levantó el servidor de desarrollo y se recorrió la página completa con
  un navegador headless (scroll incluido, para disparar las animaciones de
  aparición): sin errores de consola, texto e íconos coinciden con el
  mockup y con los 6 ajustes de las capturas.
- No se pudo probar el envío real del Google Form porque, como se explica
  en la sección 4, el formulario todavía no existe — es el único punto que
  requiere una acción manual de la clienta (crear el form) o del desarrollador
  (pegar la URL una vez creado).
