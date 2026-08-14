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
1. **Nav superior** (sticky negro): logo a la izquierda (48px desktop / 40 mobile), links **Concepto · Modelos · Contacto**. En **mobile = menú hamburguesa**.
2. **Hero ESSENTIA** (banner full-width):
   - Desktop: `essentia-hero-trio.png` (1672x941, las 3 terminaciones con la cobre al centro, `background-position:center top` para que las piletas queden abajo). La anterior (`essentia-hero.png`, 2 piletas) sigue en assets por si se vuelve. PENDIENTE: conseguir versión en alta (ideal 3344x1630 JPG/WebP ~2:1, piletas abajo y tercio superior negro) — la actual es de ChatGPT (~1672px máx) y se ablanda en 2K/4K.
   - El "Essentia" es el **wordmark oficial** `assets/essentia-wordmark.png` (4402x827, blanco transparente, sacado del vectorial `Downloads/essentia palabra.ai` pág. 13) usado como **máscara CSS** sobre el gradiente acero → letras oficiales + acabado metálico. `.hero-title` es un div con mask-image; oculto en mobile.
   - Mobile: `essentia-hero-mobile.png` (vertical, todavía con texto incrustado).
3. **"Diseño puro, funcionalidad absoluta"** + **VIDEO de producto** (`assets/video-essentia.mp4`, H.264 1920x1080 34s ~20MB, autoplay muted loop playsinline, object-fit:contain). Reemplazó al modelo 3D (model-viewer + `670_Gris.glb` + JS de rotación por scroll, todo eliminado del HTML; el .glb sigue en assets). Ya no se carga el script CDN de model-viewer.
4. **"La familia Essentia completa"** — banner FOTO full-width (`familia-essentia-3.jpg`, 2480x1772 en alta: toda la línea con la rose al centro; versiones previas siguen en assets), con el **título adentro de la foto** (`.familia-title`, top 8%) y el isologo chiquito arriba-izq. Sin crossfade (se probó alternar con `familia-foto.png` y el usuario lo descartó). Los videos `familia-*.mp4` ya no se usan.
5. **"Conocé cada detalle"** — barra de **2 opciones** (Modelos y colores · Modos de instalación), el panel se despliega debajo:
   - **Modelos y colores (fusionados):** pills de modelo **640 → 660 → 670 → 671** (de la más chica a la más grande; la **620 se SACÓ de la landing** a pedido del usuario ago 2026 — si vuelve, ojo: es MÁS GRANDE que la 640, el orden por tamaño no es el numérico) + swatches de terminación que muestran SOLO los colores disponibles del modelo elegido: 640/670/671=S+B; 660=S+B+R (S=satinado, B=negro grafito, R=ROSE — nunca decir "cobre", siempre "rose"). Fotos `model-<n>.jpg` (satinadas) y `model-<n>B/R.jpg` en assets, todas renders "con negro" 3072px. Debajo de los pills hay un **indicador de medida de CUBA** (`.t3-panel .model-size`, solo el número: 640=40×40, 660=60×40, 670=70×40, 671=71×40 cm) que sigue al modelo; OJO: va calificado con `.t3-panel` porque `.t3-panel p` le pisa el margen por especificidad. Al cambiar a un modelo sin el color elegido, cae a satinado. Ciclo automático: recorre cada modelo pasando por TODOS sus colores y recién ahí salta al siguiente (4s por paso), arrancando en la 660 rose; la rose es la terminación predeterminada al clickear un modelo que la tenga. Se frena con el primer click del usuario. Los viejos `color-gris/negra.png` del tab Colores ya no se usan.
   - **Instalación:** pills (Enrasada / Bajo mesada / Sobre mesada) + placeholder de imagen.
6. **Accesorios** — "Todo lo que necesitás, incluido" + **5 cards en una sola fila** (grid 5 col; en ≤900px la 5ª ocupa la fila completa) con fotos **transparentes**:
   Barra escurridora (`acc-barra.png`), Tabla de vidrio (`acc-tabla-1-sinfondo.png`, sangra del borde derecho de la card), Dosificador (`acc-dosificador.png`), Cesto escurridor (`acc-cesto.png`), Rejilla flexible (`acc-rejilla.png`, descripción inventada a pedido) + botón Comprar en Mercado Libre.
7. **"Essentia desde adentro"** (sección Modelos, id `#griferias-glow`) — **planos técnicos**. Acordeón **640 → 671** (chica a grande); al abrir cada modelo muestra su plano (PNG transparente líneas blancas): `plano-640/660/670/671.png`.
8. **"Dónde comprar"** — **mapa dark flotante**. Escribís una zona/dirección → muestra **un solo distribuidor** (el más cercano) + **un pin** metálico. Datos DEMO (Palermo, Belgrano, Caballito, Recoleta). Mapa estilizado (NO es Google Maps real).
9. **CTA final** "Compra segura" + bloque de **Garantía de 10 Años** (texto AISI 304-18/8 y 18/10) + botones Comprar en Mercado Libre y Dónde comprar.
10. **Footer**.

## Pendientes / TODO
- [ ] **Comprimir**: `video-essentia.mp4` (~20 MB, en uso) y `catalogo-essentia.pdf` (17,8 MB) para que carguen más rápido en celular.

- [ ] **Distribuidores reales** del mapa por zona (hoy son datos de ejemplo). Opcional: Google Maps real (necesita API key).
- [ ] Reemplazar el **placeholder de imagen de "Modos de instalación"** (Conocé cada detalle) y videos que sigan como placeholder.
- [ ] Definir si Accesorios lleva **1 o 2 fotos por card** (hoy 1; la idea original era satinada + black).

## Notas técnicas (importante para no romper)
- **Padding de títulos:** `.section` usa `padding-block` (no `padding` shorthand) para no pisar el padding horizontal de `.container`. Si se vuelve a `padding: X 0`, los títulos tocan los bordes.
- **Medidas:** el mapeo es directo (data-model 640 → `model-640.jpg`, etc.) con las fotos oficiales; el viejo cruce 660/670 de los `.png` ya no aplica.
- **Master de video:** el `.mov` original está en **ProRes** → NO reproduce en navegadores; siempre usar export **H.264 .mp4**.
- **Botones flotantes** (z-index 900, bajo el gate 9999): `.float-cat` = pill blanca centrada abajo que descarga `assets/catalogo-essentia.pdf` (17,8 MB, el PDF oficial "ESSENTIA diseño puro funcionalidad absoluta"); `.float-wsp` = burbuja outline (44px, ícono blanco) abajo a la derecha con el número **5491128563001**, configurado en la constante `WA_NUMERO` del script al final del body (si se vacía, el botón se oculta solo; nunca inventar un número). Ambos flotantes comparten el mismo estilo: vidrio oscuro + borde blanco, hover invierte a blanco lleno.

## Cómo pedir cambios en la próxima sesión
Mencionar que el proyecto es el HTML `signature-series.html` en `Downloads/Cloud`, con esta minuta como referencia.
