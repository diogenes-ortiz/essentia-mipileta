# Essentia — estado del proyecto (traspaso a sesión nueva)

> Pegá este archivo entero como primer mensaje en el chat nuevo.
> Detalle técnico completo: `Cloud/essentia-web/MINUTA-essentia.md` (leerla antes de tocar nada).

## 1. Qué es y dónde vive

Landing de la **línea Essentia** (piletas de cocina de acero inox de Mi Pileta).

| Cosa | Dónde |
|---|---|
| **Fuente única de verdad** | `C:\Users\UX534FAC\Downloads\Cloud\signature-series.html` (editar SIEMPRE acá) |
| Assets | `Cloud\assets\` |
| Versión GitHub Pages | https://diogenes-ortiz.github.io/essentia-mipileta/ — **clave `essentia2026`** |
| Carpeta del repo | `Cloud\essentia-web\` (`index.html` = copia del HTML) |
| Versión Shopify | **`mipileta.com.ar/pages/essentia` — PUBLICADA** (25/8/2026, visible al público) |
| Tienda / tema | `mipileta.myshopify.com`. **EL TEMA EN VIVO CAMBIA**: al 25/8/2026 es **"Home nueva (borrador)" #191148360044**; Horizon #181549990252 quedó despublicado. Verificar SIEMPRE con `shopify theme list` antes de publicar. |
| Respaldo del tema | `Cloud\backup-tema-horizon\` (bajado antes de tocar producción) |

## 2. Cómo publicar (los dos lados, siempre juntos)

```bash
cd /c/Users/UX534FAC/Downloads/Cloud
# 1) GitHub Pages
cp signature-series.html essentia-web/index.html
cp assets/<los nuevos> essentia-web/assets/
cd essentia-web && git add -A && \
  git -c user.name="diogenes-ortiz" -c user.email="diogenessortiz@gmail.com" commit -m "..." && \
  git push origin master && cd ..
# 2) Shopify (regenera el Liquid y sube SOLO lo indicado, sin borrar nada)
node build-essentia-shopify.js
# OJO: 191148360044 es el tema EN VIVO al 25/8/2026 (antes era 181549990252).
# Verificar con `shopify theme list` y, ante la duda, subir a los dos.
shopify theme push --theme 191148360044 --store mipileta.myshopify.com \
  --path essentia-shopify --allow-live --nodelete \
  --only "templates/page.essentia.liquid" --only "assets/*"
```

**Ver en local:** `npx -y serve -l 4321 .` en `Cloud/` → `http://localhost:4321/signature-series.html`
(para saltear el gate: `sessionStorage.setItem('essentia_ok','1')` + reload).

## 3. Reglas del cliente (NO negociables)

- La terminación rose se llama **"rose"**, nunca "cobre".
- El tratamiento de color es **"PVD"**, nunca "PDB".
- La pieza del medio del desagüe es el **"cestillo"**, nunca "filtro": no cuela, **sella la
  pileta y la deja hermética**.
- Piletas siempre **de la más chica a la más grande**: 640 → 660 → 670 → 671.
- **La 620 está fuera** de la landing (sus fotos siguen en assets por si vuelve).
  Si vuelve, ojo: **la 620 es más grande que la 640**, el orden por tamaño NO es el numérico.

## 4. Trampas técnicas (ya me mordieron, no repetirlas)

1. **Liquid interpreta `{% … %}` aunque esté dentro de comentarios JS/HTML** → si el HTML
   fuente los menciona, la página de Shopify no compila.
2. **Shopify rechaza `.mp4` y `.pdf`** en assets del tema: el video se sirve desde GitHub
   Pages; el catálogo vive en Google Drive.
3. **No usar `100vw`** para elementos a todo el ancho: incluye la barra de scroll y descentra.
   Usar `var(--vw)` / `var(--fixoff)` (los calcula el JS del final).
4. **`overflow-x` solo en `html`** (con `clip`). Si el `body` también lo lleva, aparece una
   **segunda barra de scroll**.
5. **`.t3-panel p` pisa márgenes por especificidad**: las reglas propias van calificadas
   (`.t3-panel .model-size`) y con `margin: X auto 0` (sin el `auto` se van a la izquierda).
6. **Fotos que alternan**: hay que normalizar el PAR junto (misma escala + centrado en lienzo
   idéntico), si no el objeto salta al fundir. Chequeo: superponer las dos al 50% → si
   coinciden, se ve una sola pieza nítida.
7. **El navegador de pruebas (Browser pane) miente**: screenshots negros, animaciones y
   timers congelados, viewport en 0, resize que no dispara. Verificar por DOM
   (`getBoundingClientRect`, `getComputedStyle`) y forzar `getAnimations().finish()`.
8. Al reemplazar una imagen con el mismo nombre, **subir el `?v=N`** (cache-bust).
9. **`flex-basis` cambia de eje al pasar a columna**: `flex:1 1 260px` pensado como ANCHO se
   vuelve ALTURA cuando el media query pone `flex-direction:column`. Así el campo del
   newsletter quedaba de 260 px de alto en mobile. Se corrige con `flex:0 0 auto`.
10. **Inputs en mobile: `font-size:16px` exactos.** Con menos, iOS hace zoom al enfocar.
11. **GitHub Pages corre Jekyll y falla en silencio.** Jekyll interpreta Liquid en los
    `.md` y `.html` del repo. La trampa 1 de esta lista, escrita como texto en el handoff,
    alcanzó para romper el build: Pages devolvía "Page build failed", **el push salía bien**
    y el sitio seguía sirviendo la última versión que compiló, sin ningún aviso.
    Ya está el archivo vacío **`.nojekyll`** en la raíz del repo, que desactiva Jekyll.
    **No borrarlo.** Si alguna vez la web no muestra los cambios aunque el push haya
    andado, revisar el build antes que cualquier otra cosa:
    `gh api repos/diogenes-ortiz/essentia-mipileta/pages/builds?per_page=5`
    (el campo `status` tiene que decir `built`, no `errored`).

12. **El tema en vivo puede cambiar y la página se cae sin aviso.** El 25/8/2026 se publicó
    el tema nuevo de la home y Horizon quedó despublicado. Como `page.essentia.liquid` solo
    existía en Horizon, `mipileta.com.ar/pages/essentia` empezó a dar **404** — con la página
    marcada como `isPublished: true`, lo cual despista. A Zíngara le pasó lo mismo.
    **Antes de cada push: `shopify theme list --store mipileta.myshopify.com`** y mirar cuál
    dice `[live]`. Conviene subir el template a los dos temas, por si vuelven a rotar.

## 5. Estado de cada sección

1. **Nav** sticky: Concepto · Modelos · Compra segura · Contacto (`scroll-margin-top:88px`).
   **El logo vuelve a la home** (`mipileta.com.ar`): antes era un `<img>` suelto y desde la
   landing no había forma de volver. El `<a class="logo-link">` lleva `line-height:0`, si no le
   suma unos px de alto a la barra.
   **Contacto abre WhatsApp** (antes iba al mapa, que quedó oculto); el href se arma en el
   mismo script de `WA_NUMERO`, así el número está en un solo lugar.
2. **Hero**: foto `essentia-hero-trio.png` + wordmark oficial (máscara CSS). **Video encima**
   que arranca **6 s después del primer movimiento de mouse** (o touch/scroll/teclado),
   **desde el segundo 4.4** (en 4.0 el video está en fundido a negro), en loop que vuelve a
   4.4. Constantes `ESPERA` y `DESDE` en el script del final.
   **Al entrar el video el wordmark se difumina y se va** (animación `ess-title-out`, 1,2 s:
   opacidad + sube 18 px + blur). Va como animación y no como transition porque la de
   entrada es `both` y le pisaría la opacidad.
   Mobile: banner vertical propio `essentia-hero-mobile-trio.png`.
   **Es el único video de la página** (ago 2026): la sección "Diseño puro, funcionalidad
   absoluta" lo repetía y se sacó entera, con su CSS y su script.
3. **"La familia Essentia completa"**: banner `familia-essentia-3.jpg` con el título adentro.
4. **"Conocé cada detalle"**: tres tabs.
   - *Modelos y colores*: pills + swatches por modelo (640/670/671 = satinado+negro; 660 suma
     rose) + medida de **cuba** debajo. Ciclo automático que recorre cada modelo por todos sus
     colores, arrancando en la **660 rose**.
   - *Instalación*: **esquema SVG en corte dibujado por código** (mesada tramada vs pileta de
     acero) con los 3 modos + link "Ver tutorial" que **cambia de video de YouTube según el modo**.
   - *Desagüe*: despiece apilado (tapa + **cestillo** + cuerpo) en `desague-essentia.jpg`, recortado
     1:1 y con marco cuadrado (`.inst-media--cuadrada`). El texto lo redactamos nosotros:
     falta que lo valide el cliente.
5. **"Essentia desde adentro"**: planos técnicos en acordeón.
   *(Se movió acá, antes de Accesorios: primero se termina de hablar de las piletas.)*
6. **Complementos** (id interno `#accesorios`): eyebrow "Complementos", título "Todo lo que
   necesitás, a mano". **Nunca decir "incluido"** (ago 2026): hacía pensar que venían de
   regalo con la pileta, y se venden aparte.
   **6 cards** (barra, tabla, cesto, rejilla, box de residuos, dosificador), fotos recortadas
   al contenido. Grilla de **3 columnas** en desktop (2 filas parejas) y 2x3 en mobile.
   **Cada card va al detalle del producto en Shopify** con hover (se eleva + "Ver en catálogo →"):
   barra→`barra-escurridora-compacta-50`, tabla→`tablas-de-vidrio`,
   cesto→`cestos-escurridores`, rejilla→`rejilla-flexible`, box→`box-inox`,
   dosificador→`dosificador-1004` (es el de **cabezal cuadrado**; el 1000 lo tiene redondeado).
   Las fotos del box y del dosificador venían con **fondo blanco**: se recortaron con relleno
   por inundación **desde los bordes**. No se puede filtrar "todo lo blanco" porque la botella
   del dosificador ES blanca y se borraría; solo es fondo lo blanco conectado al borde.
   **PENDIENTE:** la ficha del Dosificador 1004 en Shopify lista piletas compatibles y **no
   incluye los modelos Essentia**. Puede ser una lista vieja: confirmarlo con el cliente.
   Cesto y rejilla **alternan satinado↔color** (a la vez, cada 2,8 s).
7. **"Dónde comprar"**: mapa con distribuidores **DEMO** — **OCULTO** (`display:none`).
   Para reactivarlo: sacar el `display:none` y devolver el botón "Dónde comprar" al CTA.
8. **CTA "Compra segura"** + garantía de 10 años. **Sin la nota de Mercado Libre**: estaba
   repetida con la de Complementos, que es la única que queda en la página.
9. **Newsletter**: título "Recibí las últimas novedades" (general, no "Novedades de Essentia").
   4 perfiles en pills (Para mi casa · Arquitectura · Mueblería · Instalación).
    En GitHub solo valida y agradece; en Shopify se convierte en alta de cliente y **el perfil
    viaja como etiqueta** para segmentar.
10. **"El mismo acero, en el baño"**: cierre con botón outline **"Conocé la línea Zíngara"**
    → `diogenes-ortiz.github.io/zingara-mipileta` (tiene gate, clave `zingara2026`).
11. **Footer** + botones flotantes: catálogo (Google Drive) y WhatsApp (+54 9 11 2856-3001),
    ambos outline; el de catálogo queda **clavado al desagüe** de la foto del hero.

## Mobile (revision ago 2026)

La pagina bajaba **18 MB** en celular. Ahora baja **1,5 MB**. Que se hizo:

- **El video NO va en mobile.** El hero de celular es vertical (941x1672) y el video es
  16:9: con object-fit:cover se veia apenas el **32% del ancho**, muy ampliado. El script
  saca el <video> del DOM cuando matchMedia("(max-width:640px)") da true, asi ni se descarga
  (6,3 MB). En desktop sigue igual.
- **preload="none" + v.load() en la primera interaccion.** El video recien se ve 6 s despues,
  asi que no tiene sentido bajarlo durante la carga inicial compitiendo con la foto del hero.
- **Imagenes reducidas: 12,4 MB -> 2,4 MB.** Los originales quedaron en `Cloud/assets-originales/`
  (fuera del repo). Detalle:
  - `model-*.jpg` x9: 3072px -> **1200px** (se muestran a 560px como maximo).
  - `acc-*.png` x6 -> **`acc-*.webp`** (1493 KB -> 449 KB). Mantienen transparencia.
  - `familia-essentia-3.jpg`: 2480px -> 1800px (3054 KB -> 190 KB).
  - `essentia-hero-trio.png` -> **`.jpg`** (782 -> 76 KB) y `essentia-hero-mobile-trio.png`
    -> **`.jpg`** (1094 -> 106 KB). Mismas dimensiones, solo cambia el formato.
    **OJO:** al renombrarlos hay que tocar las claves del objeto `FOTOS` (el que alinea el
    boton de catalogo al tapon). Sus `w`/`h`/`tapon` son un sistema de coordenadas, no
    pixeles reales: al redimensionar o recomprimir NO se tocan.
  - `mp-sintesis.png`: 2000px -> 160px (se muestra a 33px).
- **Areas tactiles de 44px**: los pills estaban en 36 y los swatches en 26.
  El bloque `@media (hover:none)` va **al final del <style>** a proposito: empata en
  especificidad con reglas de mas arriba y solo gana por orden de aparicion.
- **Textos legibles**: la descripcion de las cards estaba en 11,2px (`.7rem`), ahora `.82rem`.
- **La grilla de complementos alinea con el .container** (antes sobresalia 5px de cada lado).
- **Titulos centrados en mobile**: "Conoce cada detalle" y "Essentia desde adentro" van a la
  izquierda por estilo inline y en pantalla angosta desentonaban.

## Cómo publicar / despublicar la página en Shopify

El CLI de temas NO maneja páginas: se hace con el Admin API vía `shopify store execute`.

**Ojo con el dominio.** El permanente de la tienda es **`v351rf-0f.myshopify.com`**.
`mipileta.myshopify.com` sirve para `theme push`, pero `store auth` lo rechaza con
"OAuth callback store does not match". Son la misma tienda: `v351rf-0f` redirige a
`mipileta.com.ar`. La página es `gid://shopify/Page/711251525996`.

Ver el estado:

```bash
shopify store execute -s v351rf-0f.myshopify.com --json -q "{ pages(first: 5, query: \"handle:essentia\") { nodes { id handle isPublished } } }"
```

Publicar (o `false` para volver a ocultarla):

```bash
shopify store execute -s v351rf-0f.myshopify.com --json --allow-mutations -q "mutation P($id: ID!) { pageUpdate(id: $id, page: { isPublished: true }) { page { handle isPublished } userErrors { field message } } }" -v "{\"id\":\"gid://shopify/Page/711251525996\"}"
```

## URLs cortas (redirecciones de Shopify)

La tienda usa redirecciones 301 para tener URLs lindas. Se crean con el Admin API,
igual que la publicación de la página (ver más arriba el tema del dominio permanente):

| URL corta | Va a |
|---|---|
| `mipileta.com.ar/essentia` | `/pages/essentia` (creada 25/8/2026) |
| `mipileta.com.ar/zingara` | `/pages/zingara` |
| `mipileta.com.ar/garantia` | `/pages/garantia` |

Funcionan sin distinguir mayúsculas y con barra final. Para crear otra:

```bash
shopify store execute -s v351rf-0f.myshopify.com --json --allow-mutations -q "mutation C($r: UrlRedirectInput!) { urlRedirectCreate(urlRedirect: $r) { urlRedirect { id path target } userErrors { field message } } }" -v "{\"r\":{\"path\":\"/loquesea\",\"target\":\"/pages/loquesea\"}}"
```

## 6. Pendientes abiertos

**Esperando respuesta del cliente**
- [x] ~~Cuál de las 4 barras escurridoras es la de la foto~~ → es la **compacta 50 satinada**
      (se identificó contando ranuras: la nuestra tiene 7 por panel, la 60 tiene 8). Confirmar
      con el cliente si aparece una foto que lo contradiga.
- [x] ~~La rejilla flexible no existe como producto~~ → **sí existe** (`rejilla-flexible`), ya enlazada.
- [ ] ¿Reponer el **dosificador**? Se sacó porque no estaba entre las fotos nuevas.
- [x] ~~**Publicar la página de Shopify**~~ → **publicada el 25/8/2026**. Verificado en vivo:
      HTTP 200, sin gate de contraseña (el build lo elimina y aborta si quedan restos), las 23
      imágenes cargan desde el CDN de Shopify, y el newsletter quedó como **form nativo**
      (`form_type=customer`, `action=/contact`) con la etiqueta viajando bien
      (`newsletter, <perfil>`, comprobado cambiando de pill sin enviar nada).
      **Falta la suscripción de prueba real:** crea un cliente en la tienda del cliente, así
      que conviene hacerla a mano o pedirla explícitamente.

**Técnicos**
- [ ] **Hero en alta resolución** (el actual es 1672px, se ablanda en 2K/4K). Ideal ~3344x1630,
      JPG/WebP, piletas abajo y tercio superior negro.
- [ ] **Distribuidores reales** del mapa (hoy son de ejemplo: Palermo, Belgrano, Caballito,
      Recoleta). **La sección está OCULTA** (`display:none`) hasta tener los datos reales.
- [ ] **Barra escurridora en negro** con el MISMO encuadre que la satinada, para que también alterne.
- [ ] Link real de **Mercado Libre** (hoy es una nota de texto porque no hay a dónde mandar).
- [x] ~~El botón final "Conocé la línea Zíngara" apunta a GitHub Pages (con gate)~~ →
      **resuelto el 25/8/2026**: la landing de Zíngara ya está publicada en Shopify, así que el
      botón va a `mipileta.com.ar/zingara`. Importaba: con Essentia pública, el botón mandaba
      a un muro de contraseña.

## 7. Herramientas útiles ya instaladas

- **ffmpeg**: `npm i ffmpeg-static` en el scratchpad → binario en `node_modules/ffmpeg-static/ffmpeg.exe`.
  Receta de compresión usada: `-an -crf 27 -preset medium -movflags +faststart`.
- **@napi-rs/canvas** en el scratchpad, para medir/recortar/normalizar imágenes por código.
