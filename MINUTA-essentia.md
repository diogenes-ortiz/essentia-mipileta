# Minuta — Landing "Essentia" (Mi Pileta)

## Qué es
Landing page de la **línea Essentia** (piletas de cocina de acero inox de *Mi Pileta*).
Partió como réplica del template **Signature Series de Johnson Acero** y se adaptó 100% a Essentia
(textos sacados del PDF "ESSENTIA diseño puro funcionalidad absoluta").

## Archivo y cómo correrlo
- **HTML único:** `C:\Users\UX534FAC\Downloads\Cloud\signature-series.html`
- **Assets (imágenes/videos/3D):** `C:\Users\UX534FAC\Downloads\Cloud\assets\`
- El **logo** (`MP_Isotipo_Principal_Blanco.png`) va **incrustado en base64** dentro del HTML.
- Para verlo:
  - Servidor: en `Cloud/` correr `npx -y serve -l 4321 .` → `http://localhost:4321/signature-series.html`
  - O abrir directo: `file:///C:/Users/UX534FAC/Downloads/Cloud/signature-series.html`
- **Necesita internet** solo para las fuentes de Google (el 3D por CDN ya no se usa). Todo lo demás es local.

## Diseño / estilo
- Fondo negro, texto blanco. Fuentes: **Hanken Grotesk** (cuerpo) + **Raleway** (hero).
- Botones **pill blanco y negro**: `.btn-metal` (el nombre quedó por herencia) = pill blanca con texto negro, hover a gris `#c2c8ce`; `.btn-outline` = pill sin fondo con borde, hover invierte a fondo blanco + texto negro. Se eliminó el gradiente metálico y todo el verde agua del sitio.
- Mobile optimizado (media queries ≤640 / ≤380 / ≤700 px).

## Secciones (en orden)
1. **Nav superior** (sticky negro): logo a la izquierda (48px desktop / 40 mobile), links **Concepto · Modelos · Compra segura · Contacto** (ago 2026: **Contacto abre WhatsApp**; antes iba al mapa de distribuidores, que quedó oculto) (los targets llevan `scroll-margin-top:88px` para que el nav sticky no tape el título). En **mobile = menú hamburguesa**.
2. **Hero ESSENTIA** (banner full-width):
   - Desktop: `essentia-hero-trio.png` (1672x941, las 3 terminaciones con la cobre al centro, `background-position:center top` para que las piletas queden abajo). La anterior (`essentia-hero.png`, 2 piletas) sigue en assets por si se vuelve. PENDIENTE: conseguir versión en alta (ideal 3344x1630 JPG/WebP ~2:1, piletas abajo y tercio superior negro) — la actual es de ChatGPT (~1672px máx) y se ablanda en 2K/4K.
   - La foto va en `.hero::before` (para animarla sin mover el wordmark), con `background-size:cover`. `essentia-hero-trio.png` se actualizó ago 2026 (piletas más grandes); el tapón quedó en x=831 → hay que mantener `TAPON_X` en sincronía.
   - **Video sobre la foto** (`.hero-video`, el mismo `video-essentia.mp4` de la sección 3: el navegador lo descarga una sola vez): arranca **6s después del primer movimiento del mouse** (o touch/scroll/teclado, porque en celular no hay mouse), desde el **segundo 4.4** (en 4.0 el video está en fundido a negro), en loop que vuelve a 4.4 y no a 0. Constantes `ESPERA` y `DESDE` en el script del final.
     **Al entrar el video el wordmark se difumina y se va** (`ess-title-out`: opacidad, sube
     18px, escala 1.04 y blur 7px, 1,2 s). Va como ANIMACIÓN y no como transition, porque
     la de entrada (`ess-title-in`) es `both` y le pisaría la opacidad a cualquier transición.
     La regla es `html.js .hero-title.fuera` (0,3,1) para ganarle a `html.js .hero-title`.
     Las dos listas de `filter` llevan las MISMAS funciones, si no el blur no interpola.
   - **El archivo:** `assets/video-essentia.mp4` (H.264 1920x1080, 1:56, **6,3 MB**, comprimido
     con ffmpeg desde 70 MB: `-an -crf 27 -preset medium -movflags +faststart`; sin audio porque
     va muted). **ago 2026: es el ÚNICO video de la página.** Antes había una sección
     "Diseño puro, funcionalidad absoluta" que lo repetía; se sacó entera, junto con su CSS
     (`.hero-3d`, `.model3d`) y su script. Esa sección venía de reemplazar al modelo 3D
     (model-viewer + `670_Gris.glb`), ya eliminado; el .glb sigue en assets sin uso.
     **Ojo:** con eso se fue de la página el claim "Diseño puro, funcionalidad absoluta".
   - El "Essentia" es el **wordmark oficial** `assets/essentia-wordmark.png` (4402x827, blanco transparente, sacado del vectorial `Downloads/essentia palabra.ai` pág. 13) usado como **máscara CSS** sobre el gradiente acero → letras oficiales + acabado metálico. `.hero-title` es un div con mask-image.
   - **Mobile:** banner vertical propio `essentia-hero-mobile-trio.png` (941x1672, las 3 terminaciones apiladas) con su `aspect-ratio` nativo (941/1672) para que entren completas, + el mismo wordmark (width min(58vw,240px), top 3.5%; la primera pileta arranca en el 11,8% del alto, quedan ~14px de aire). La vieja `essentia-hero-mobile.png` (con texto incrustado) quedó sin uso.
3. **"La familia Essentia completa"** — banner FOTO full-width (`familia-essentia-3.jpg`, 2480x1772 en alta: toda la línea con la rose al centro; versiones previas siguen en assets), con el **título adentro de la foto** (`.familia-title`, top 8%) y el isologo chiquito arriba-izq. Sin crossfade (se probó alternar con `familia-foto.png` y el usuario lo descartó). Los videos `familia-*.mp4` ya no se usan.
4. **"Conocé cada detalle"** — barra de **2 opciones** (Modelos y colores · Modos de instalación), el panel se despliega debajo:
   - **Modelos y colores (fusionados):** pills de modelo **640 → 660 → 670 → 671** (de la más chica a la más grande; la **620 se SACÓ de la landing** a pedido del usuario ago 2026 — si vuelve, ojo: es MÁS GRANDE que la 640, el orden por tamaño no es el numérico) + swatches de terminación que muestran SOLO los colores disponibles del modelo elegido: 640/670/671=S+B; 660=S+B+R (S=satinado, B=negro grafito, R=ROSE — nunca decir "cobre", siempre "rose"). Fotos `model-<n>.jpg` (satinadas) y `model-<n>B/R.jpg` en assets, todas renders "con negro" 3072px. Debajo de los pills hay un **indicador de medida de CUBA** (`.t3-panel .model-size`, solo el número: 640=40×40, 660=60×40, 670=70×40, 671=71×40 cm) que sigue al modelo; OJO: va calificado con `.t3-panel` porque `.t3-panel p` le pisa el margen por especificidad. Al cambiar a un modelo sin el color elegido, cae a satinado. Ciclo automático: recorre cada modelo pasando por TODOS sus colores y recién ahí salta al siguiente (4s por paso), arrancando en la 660 rose; la rose es la terminación predeterminada al clickear un modelo que la tenga. Se frena con el primer click del usuario. Los viejos `color-gris/negra.png` del tab Colores ya no se usan.
   - **Instalación:** pills (Enrasada / Bajo mesada / Sobre mesada) + **esquema SVG en corte dibujado por código** (`.inst-scheme`): un `<g class="inst-fig">` por modo que se funden entre sí (crossfade + leve desplazamiento). La **mesada** va con `<pattern>` de tramado diagonal gris y la **pileta** con trazo `stroke:url(#acero)` (degradé metálico) — así se distinguen. Incluye referencia de colores y una descripción que cambia con el modo. Geometría: superficie de mesada en y=112, canto inferior en y=138; el ala de la pileta apoya sobre 112 (sobremesada), va al ras en 112 (enrasada) o cuelga en 141 (bajo mesada). Debajo, link minimalista **"Ver tutorial de instalación"** (`.tuto-link`) que **cambia de video según el modo** (objeto `TUTORIALES` en el JS): enrasada→`g3jLrV_NTKI` ("colocación de productos"), bajo→`MLWkF-hHZaI` ("bajomesada"), sobre→`WPoPcBXMASI` ("de encastre"). La asignación se dedujo de los títulos del canal; confirmar con el usuario si aparece uno más específico para enrasada. OJO: `.inst-scheme svg{width:100%}` pisa el tamaño del ícono, por eso va calificado `.inst-scheme .tuto-link svg`. Ya no se usa `assets/instalacion.jpg`.
5. **"Essentia desde adentro"** — *(ago 2026: se movió ACÁ, justo después de "Conocé cada
   detalle", porque primero se termina de hablar de las piletas y recién después vienen
   los accesorios. Antes colgaba dentro de `#signature` con dos `</div>` de más, y por eso
   la nota de Mercado Libre se escapaba del `.container` y perdía el padding.)* (sección Modelos, id `#griferias-glow`) — **planos técnicos**. Acordeón **640 → 671** (chica a grande); al abrir cada modelo muestra su plano (PNG transparente líneas blancas): `plano-640/660/670/671.png`.
6. **Complementos** (id interno `#accesorios`) — eyebrow **"Complementos"** + título **"Todo lo que necesitás, a mano"**. Antes decía "Accesorios" y "…incluido"; el cliente lo cambió (ago 2026) porque **"incluido" hacía pensar que venían de regalo con la pileta**, y se venden aparte. NO volver a esa palabra. Es + **4 cards en una fila** (grid 4 col; 2x2 en ≤900px) con las **fotos oficiales** de `Downloads/Accesorios Web/` (PNG 3072px sin fondo): Barra escurridora (`acc-barra.png`), Tabla de vidrio (`acc-tabla.png`), Cesto escurridor (`acc-cesto.png`), Rejilla flexible (`acc-rejilla.png`) + **nota** `.ml-nota` (texto, NO botón): "Encontranos en Mercado Libre a través de nuestros distribuidores". Se sacaron los dos botones "Comprar en Mercado Libre" (ago 2026) porque **no hay link real a dónde mandar**; un botón que no lleva a ningún lado frustra. Cuando exista el link, vuelve a ser botón.
   - Las originales traen el accesorio ocupando solo 14-25% del cuadro: el script del scratchpad las **recorta al contenido** (+3% de margen) y las baja a 1000px, y `.acc-img` va con `padding:0` → el accesorio ocupa 86-93% de la card ("plano importante", pedido del usuario ago 2026).
   - **El dosificador se sacó** (no estaba entre las fotos nuevas). Su foto vieja sigue en assets.
   - **Dos terminaciones por card con crossfade** (`.acc-dual` + `.acc-frame`, fundido 0,7s, un único `setInterval` de 2,8s: **cambian a la vez**): cesto satinado↔rose y rejilla satinada↔negra.
   - **Los pares se normalizan juntos** (script en el scratchpad): se mide el objeto en las dos fotos, se llevan a la misma escala y se centran en un lienzo de 1000x1000. Si se recorta cada foto por su cuenta, **el objeto salta de lugar al fundir** (le pasó al usuario, ago 2026). Para chequear: superponer las dos al 50% — si coinciden, se ve una sola pieza nítida.
   - **ago 2026: el hover dice "Ver en catálogo" y cada card va al DETALLE del producto**:
     barra→`/products/barra-escurridora-compacta-50`, tabla→`/products/tablas-de-vidrio`,
     cesto→`/products/cestos-escurridores`, rejilla→`/products/rejilla-flexible`.
     La **rejilla flexible SÍ existe** como producto (se creyó que no). La **barra es la
     "compacta 50" satinada**: se identificó contando ranuras contra las fotos de la tienda
     (la nuestra tiene 7 por panel, la "60" tiene 8); las otras dos variantes son negras.
   - **Cada card es un link a la tienda** (`a.product`, abre en pestaña nueva) con hover: la card se eleva, la foto crece 5% y aparece "Ver en la tienda →" (en táctil el aviso queda fijo). Tabla→`/products/tablas-de-vidrio`, cesto→`/products/cestos-escurridores`; **barra y rejilla van a la colección** `?filter.p.vendor=Accesorios` porque de barra hay 4 variantes (50/60, acero/negra) y no se sabe cuál es la de la foto, y la rejilla no existe como producto en la tienda. PENDIENTE confirmar con el usuario.
   - La **tabla de vidrio** se aclaró (gamma 0.62 + piso de luz): pasó de luminancia media 6 a 34, porque en negro sobre negro se perdía.
   - **La barra NO alterna**: sus dos fotos son tomas con encuadres distintos (no el mismo render con otro color), así que no hay forma de alinearlas. Queda con la satinada. Si aparece la negra con el mismo encuadre, se suma.
   - La **tabla** también queda con una sola (sus dos fotos son casi idénticas).
7. **"Dónde comprar"** — **OCULTO (ago 2026, pedido del cliente)**: la sección sigue en el
   HTML con `style="display:none"` porque los distribuidores son de ejemplo. Para volver a
   mostrarla: sacar el `display:none` y devolver el botón "Dónde comprar" al CTA de compra
   segura. **Ojo:** el "Contacto" del nav apuntaba a este mapa; ahora abre **WhatsApp**, y el
   href se arma en el mismo script de `WA_NUMERO` para no repetir el número.
   Era un **mapa dark flotante**. Escribís una zona/dirección → muestra **un solo distribuidor** (el más cercano) + **un pin** metálico. Datos DEMO (Palermo, Belgrano, Caballito, Recoleta). Mapa estilizado (NO es Google Maps real).
8. **CTA final** `#compra-segura` (enlazada desde el menú) "Compra segura" + bloque de **Garantía de 10 Años** (texto AISI 304-18/8 y 18/10) y la nota `.ml-nota` de los distribuidores.
9. **Newsletter** (`#newsletter`, al final) — título **"Recibí las últimas novedades"** (ago 2026: antes decía "Novedades de Essentia"; se generalizó a pedido del cliente). Cuatro perfiles en pills: **Para mi casa · Arquitectura · Mueblería · Instalación** (el consumidor final va por momento, no por categoría; los oficios como rubro para no etiquetar a la persona) + campo de correo. En GitHub **valida y agradece pero NO envía** (es estático); en Shopify el build lo cambia por el form nativo de altas y el perfil viaja como **etiqueta del cliente** (`newsletter, <perfil>`) para segmentar. El mismo JS sirve en los dos: si el form tiene `action` (Shopify) deja pasar el envío real. El validador de correo NO usa regex a propósito (se rompía al escapar).
10. **"El mismo acero, en el baño"** (`#otra-linea`, ago 2026) — cierre con botón outline
    **"Conocé la línea Zíngara"** → `diogenes-ortiz.github.io/zingara-mipileta` (ojo: tiene
    gate, clave `zingara2026`). Cuando Zíngara tenga página propia en Shopify, cambiar la URL.
11. **Footer**.

## Pendientes / TODO
- [ ] Opcional: borrar `assets/catalogo-essentia.pdf` (17,8 MB, ya no se usa: el catálogo está en Drive). Para comprimir videos hay receta: `npm i ffmpeg-static` en el scratchpad y usar el binario.
- [ ] **Distribuidores reales** del mapa por zona (hoy son datos de ejemplo). Opcional: Google Maps real (necesita API key).
- [ ] Definir si Accesorios lleva **1 o 2 fotos por card** (hoy 1; la idea original era satinada + black).
- [x] ~~Cuál de las 4 barras es la de la foto~~ → **compacta 50 satinada** (contando ranuras).
- [x] ~~La rejilla flexible no existe como producto~~ → **sí existe**, `rejilla-flexible`.

## Notas técnicas (importante para no romper)
- **Padding de títulos:** `.section` usa `padding-block` (no `padding` shorthand) para no pisar el padding horizontal de `.container`. Si se vuelve a `padding: X 0`, los títulos tocan los bordes.
- **Medidas:** el mapeo es directo (data-model 640 → `model-640.jpg`, etc.) con las fotos oficiales; el viejo cruce 660/670 de los `.png` ya no aplica.
- **Doble barra de scroll:** el recorte horizontal va SOLO en `html` (`overflow-x:clip`, con `hidden` de fallback). Si el `body` también lo lleva, su overflow-y pasa a `auto` y aparece una SEGUNDA barra. `clip` no crea contenedor de scroll, así que el nav sticky sigue funcionando.
- **Botón "Descargar catálogo" alineado al desagüe:** el JS del final calcula dónde cae el tapón de la pileta rose sobre el render real del background (cover + center) y posiciona el botón ahí (objeto `FOTOS`: detecta cuál está usando el hero y usa sus medidas — horizontal 1672x941 tapón 833.5 = 49,85%; vertical 941x1672 tapón 469.5 = 49,89%. NO caen en el centro exacto). Si se cambia una foto del hero, hay que volver a medir su tapón.
- **Centrado / full-bleed (NO usar `100vw`):** `100vw` incluye la barra de scroll y descentra todo (el banner familia se iba 7px). El JS del final del body setea `--vw` (ancho real del contenido) y `--fixoff` (corrección para elementos `position:fixed` centrados, medida en vivo porque cada navegador ancla distinto). Usar `var(--vw)` en los full-bleed (`.familia-banner`, `.familia-video`, `.acc-cards`) y `var(--fixoff)` en los flotantes. Se recalcula con resize + ResizeObserver.
- **Márgenes en `.t3-panel p`:** esa regla trae `margin:0 auto`; si una regla propia pone `margin:X 0 0`, el bloque pierde el centrado y queda pegado a la izquierda (le pasó al indicador de medida). Siempre `margin:X auto 0`.
- **`flex-basis` al pasar a columna (newsletter mobile):** `.news-row input` tenía
  `flex:1 1 260px`, pensado como ANCHO. En ≤640px la fila pasa a `flex-direction:column` y
  esos 260px se convierten en **ALTURA**: el campo de correo quedaba de 260px de alto y
  descuadraba toda la sección. Se corrige con `flex:0 0 auto` en el mismo media query.
- **Inputs en mobile con `font-size:16px` exactos:** con menos, iOS hace zoom al enfocar y
  descoloca la página. El campo del newsletter estaba en 14,4px.
- **Pills del newsletter en mobile:** en flex quedaban 3 arriba y 1 huérfano abajo; van en
  `grid-template-columns:1fr 1fr` para que sea un 2x2 parejo.
- **Master de video:** el `.mov` original está en **ProRes** → NO reproduce en navegadores; siempre usar export **H.264 .mp4**.
- **Botones flotantes** (z-index 900, bajo el gate 9999): `.float-cat` = pill outline centrada abajo (alineada al desagüe) que abre el **catálogo en Google Drive** (archivo público `1iskmdVyorX49qJoOP1mKNbuMj8o2RP_H`; se actualiza desde Drive sin tocar la web). El viejo `assets/catalogo-essentia.pdf` quedó sin uso; `.float-wsp` = burbuja outline (44px, ícono blanco) abajo a la derecha con el número **5491128563001**, configurado en la constante `WA_NUMERO` del script al final del body (si se vacía, el botón se oculta solo; nunca inventar un número). Ambos flotantes comparten el mismo estilo: vidrio oscuro + borde blanco, hover invierte a blanco lleno.

## Cómo pedir cambios en la próxima sesión
Mencionar que el proyecto es el HTML `signature-series.html` en `Downloads/Cloud`, con esta minuta como referencia.
