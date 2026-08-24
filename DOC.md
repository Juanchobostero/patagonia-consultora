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
├── favicon-16x16.png         # Favicon (recorte del isotipo "PC" del logo real)
├── favicon-32x32.png
├── apple-touch-icon.png      # Ícono para iOS/Safari (180×180)
├── icon-512.png              # Versión grande del isotipo (uso futuro: PWA/manifest)
├── images/
│   └── hero-neuquen.jpg      # Foto real del Lago Correntoso, Neuquén (ver sección 2)
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

### Foto real del hero (pedido de la clienta)

El fondo del hero era un paisaje ilustrado en SVG (placeholder del mockup).
Se reemplazó por una foto real con paisaje de agua, a pedido de la clienta:
**Lago Correntoso, provincia del Neuquén**, en `public/images/hero-neuquen.jpg`.

(Primero se había probado con una foto de las bardas de Plottier —sin
agua—, pero se cambió por esta a pedido explícito de mostrar "paisaje con
agua".)

- **Fuente:** Wikimedia Commons —
  [File:Lago Correntoso, Provincia del Neuquén.jpg](https://commons.wikimedia.org/wiki/File:Lago_Correntoso,_Provincia_del_Neuqu%C3%A9n.jpg)
- **Autor:** Bandurrias
- **Licencia:** CC BY-SA 4.0 (uso comercial permitido, requiere atribución).
  Por eso se agregó un crédito discreto en la esquina superior derecha de la
  foto (`.photo-credit` en `Hero.astro`/`global.css`), enlazado a la fuente.
- `Hero.astro`: se cambió el `<svg>` por un `<img>` dentro del mismo
  `.hero-photo` (mismo contenedor, mismos difuminados `::before`/`::after`
  de `global.css` — no se tocó nada más del layout).

**Pendiente/opcional:** si la clienta prefiere una foto propia (tomada por
ella, de un banco de imágenes pago, o con más "marca" del negocio — por
ejemplo con alguna referencia a Vaca Muerta/Oil & Gas), alcanza con
reemplazar `public/images/hero-neuquen.jpg` por la nueva imagen (mismo
nombre de archivo o actualizando el `src` en `Hero.astro`) y quitar/ajustar
el crédito si la nueva foto no lo requiere.

Todas las capturas se pudieron leer con claridad — no hubo ninguna anotación dudosa.

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
- Open Graph completo (`og:title`, `og:description`, `og:image`, `og:type`, `og:locale`, `og:site_name`).
- Twitter Card (`summary_large_image`).
- JSON-LD `ProfessionalService` (nombre, descripción, área de servicio, dirección en Neuquén, áreas de conocimiento).
- `robots.txt` + sitemap automático (`@astrojs/sitemap`, se genera en cada build).
- `lang="es"` en `<html>`, `theme-color` de marca.
- Imágenes con `width`/`height` explícitos para evitar layout shift.
- **Favicon con el logo real**: se recortó el isotipo "PC" del `logo.png`
  original (esquina superior izquierda del logo, sin el texto "Patagonia
  Consultora") y se generaron `favicon-16x16.png`, `favicon-32x32.png` y
  `apple-touch-icon.png`, referenciados en `Layout.astro`. Así la pestaña
  del navegador muestra el logo de la marca en vez de un ícono genérico.

**Pendiente antes de indexar en producción:**
- Confirmar el dominio final y actualizarlo en `astro.config.mjs` (`site:`) y en `src/consts.ts` (`SITE.url`) — hoy están con el placeholder `https://patagoniaconsultora.com.ar`.
- Diseñar una imagen OG dedicada de 1200×630 (hoy se reutiliza `logo.png`, que es apaisado y no ideal para redes sociales).
- Dar de alta el sitio en Google Search Console una vez esté el dominio definitivo.

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
- **Teléfono/WhatsApp de la consultora** (`299578813`): ahora
  `SOCIAL.whatsapp` en `src/consts.ts` apunta a
  `https://wa.me/549299578813` (botón flotante y footer), y se agregó
  `SITE.phoneDisplay` (`+54 299 578-813`) como link `tel:` en el footer y
  en el JSON-LD de SEO.
- **Íconos de redes en el footer**: los links de contacto del footer (antes
  texto plano) ahora son íconos circulares con el color de marca de cada
  red — ver `.footer-social` en `global.css` y `Footer.astro`.
  - **WhatsApp** (verde `#25D366`): usa el número real, `wa.me/549299578813`.
  - **LinkedIn** (azul `#0A66C2`): apunta a la página de la empresa
    (`SOCIAL.linkedin` en `src/consts.ts`,
    `linkedin.com/company/patagonia-consultora-soluciones-integrales`).
  - **Instagram**: se quitó del footer (no hay cuenta todavía). Cuando la
    creen, se vuelve a agregar el ícono siguiendo el mismo patrón que
    WhatsApp/LinkedIn en `Footer.astro`.
- **No se usó (por ahora)**: la universidad de cada profesional (UNNE /
  Universidad Nacional de Quilmes) no tiene un lugar en el diseño actual de
  las tarjetas de equipo — quedan solo nombre + rol, igual que en el
  mockup. Avisen si quieren que se agregue.

### Ya incorporado (fotos del equipo)

- **Fotos del equipo**: `Team.astro` muestra `cristina-image.webp` y
  `patricia-image.webp` (`public/images/`) en vez de las iniciales, con
  `object-fit: cover` dentro del contenedor cuadrado responsivo.

### Todavía pendiente (no vino en `datos-patagonia.txt`)

- **Instagram de la empresa**: no hay cuenta todavía; el ícono se quitó del
  footer hasta que la creen (ver nota arriba).
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
