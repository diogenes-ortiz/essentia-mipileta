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
- **Necesita internet** para: el modelo 3D (model-viewer por CDN) y las fuentes de Google. Todo lo demás es local.

## Diseño / estilo
- Fondo negro, texto blanco. Fuentes: **Hanken Grotesk** (cuerpo) + **Raleway** (hero).
- Botones **pill blanco y negro**: `.btn-metal` (el nombre quedó por herencia) = pill blanca con texto negro, hover a gris `#c2c8ce`; `.btn-outline` = pill sin fondo con borde, hover invierte a fondo blanco + texto negro. Se eliminó el gradiente metálico y todo el verde agua del sitio.
- Mobile optimizado (media queries ≤640 / ≤380 / ≤700 px).

## Secciones (en orden)
1. **Nav superior** (sticky negro): logo a la izquierda (48px desktop / 40 mobile), links **Concepto · Modelos · Contacto**. En **mobile = menú hamburguesa**.
2. **Hero ESSENTIA** (banner full-width):
   - Desktop: `essentia-hero.png` (horizontal, solo las 2 piletas, sin texto).
   - El "Essentia" es el **wordmark oficial** `assets/essentia-wordmark.png` (4402x827, blanco transparente, sacado del vectorial `Downloads/essentia palabra.ai` pág. 13) usado como **máscara CSS** sobre el gradiente acero → letras oficiales + acabado metálico. `.hero-title` es un div con mask-image; oculto en mobile.
   - Mobile: `essentia-hero-mobile.png` (vertical, todavía con texto incrustado).
3. **"Diseño puro, funcionalidad absoluta"** + **modelo 3D** (`model-viewer`, `670_Gris.glb`) que **rota + hace zoom con el scroll** (camera-orbit 115%→100%, gira ±30°; el radio ≥100% evita que se corten los bordes).
4. **"La familia Essentia completa"** — banner FOTO full-width (`familia-essentia.png`, 1783x882: toda la línea con la cobre al centro), con el **título adentro de la foto** (`.familia-title`, top 8%) y el isologo chiquito arriba-izq. Sin crossfade (se probó alternar con `familia-foto.png` y el usuario lo descartó). Los videos `familia-*.mp4` ya no se usan.
5. **"Conocé cada detalle"** — barra de **2 opciones** (Modelos y colores · Modos de instalación), el panel se despliega debajo:
   - **Modelos y colores (fusionados):** pills de modelo **640 → 660 → 670 → 671** (de la más chica a la más grande; la **620 se SACÓ de la landing** a pedido del usuario ago 2026 — si vuelve, ojo: es MÁS GRANDE que la 640, el orden por tamaño no es el numérico) + swatches de terminación que muestran SOLO los colores disponibles del modelo elegido: 640/670/671=S+B; 660=S+B+R (S=satinado, B=negro grafito, R=cobre). Fotos `model-<n>.jpg` (satinadas) y `model-<n>B/R.jpg` en assets, todas renders "con negro" 3072px. Al cambiar a un modelo sin el color elegido, cae a satinado. Ciclo automático por modelos (satinado) hasta que el usuario clickea. Los viejos `color-gris/negra.png` del tab Colores ya no se usan.
   - **Instalación:** pills (Enrasada / Bajo mesada / Sobre mesada) + placeholder de imagen.
6. **Accesorios** — "Todo lo que necesitás, incluido" + **5 cards en una sola fila** (grid 5 col; en ≤900px la 5ª ocupa la fila completa) con fotos **transparentes**:
   Barra escurridora (`acc-barra.png`), Tabla de vidrio (`acc-tabla-1-sinfondo.png`, sangra del borde derecho de la card), Dosificador (`acc-dosificador.png`), Cesto escurridor (`acc-cesto.png`), Rejilla flexible (`acc-rejilla.png`, descripción inventada a pedido) + botón Comprar en Mercado Libre.
7. **"Essentia desde adentro"** (sección Modelos, id `#griferias-glow`) — **planos técnicos**. Acordeón **640 → 671** (chica a grande); al abrir cada modelo muestra su plano (PNG transparente líneas blancas): `plano-640/660/670/671.png`.
8. **"Dónde comprar"** — **mapa dark flotante**. Escribís una zona/dirección → muestra **un solo distribuidor** (el más cercano) + **un pin** metálico. Datos DEMO (Palermo, Belgrano, Caballito, Recoleta). Mapa estilizado (NO es Google Maps real).
9. **CTA final** "Calidad y respaldo" (Comprar en Mercado Libre + Dónde comprar).
10. **Footer**.

## Pendientes / TODO
- [ ] **Comprimir videos** antes de subir a la web: `familia-mobile.mp4` (~84 MB) y `familia-essentia.mp4` (~102 MB) → idealmente 5–15 MB.
- [ ] **Distribuidores reales** del mapa por zona (hoy son datos de ejemplo). Opcional: Google Maps real (necesita API key).
- [ ] Reemplazar el **placeholder de imagen de "Modos de instalación"** (Conocé cada detalle) y videos que sigan como placeholder.
- [ ] Definir si Accesorios lleva **1 o 2 fotos por card** (hoy 1; la idea original era satinada + black).

## Notas técnicas (importante para no romper)
- **Padding de títulos:** `.section` usa `padding-block` (no `padding` shorthand) para no pisar el padding horizontal de `.container`. Si se vuelve a `padding: X 0`, los títulos tocan los bordes.
- **Medidas:** el mapeo es directo (data-model 620 → `model-620.jpg`, etc.) con las fotos oficiales; el viejo cruce 660/670 de los `.png` ya no aplica.
- **Master de video:** el `.mov` original está en **ProRes** → NO reproduce en navegadores; siempre usar export **H.264 .mp4**.
- El botón flotante de WhatsApp fue **eliminado**.

## Cómo pedir cambios en la próxima sesión
Mencionar que el proyecto es el HTML `signature-series.html` en `Downloads/Cloud`, con esta minuta como referencia.
