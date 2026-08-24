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
| Versión Shopify | `mipileta.com.ar/pages/essentia` — **la página está OCULTA** (404 para visitantes) |
| Tienda / tema | `mipileta.myshopify.com`, tema **Horizon en vivo #181549990252** (CLI ya autenticado) |
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
shopify theme push --theme 181549990252 --store mipileta.myshopify.com \
  --path essentia-shopify --allow-live --nodelete \
  --only "templates/page.essentia.liquid" --only "assets/*"
```

**Ver en local:** `npx -y serve -l 4321 .` en `Cloud/` → `http://localhost:4321/signature-series.html`
(para saltear el gate: `sessionStorage.setItem('essentia_ok','1')` + reload).

## 3. Reglas del cliente (NO negociables)

- La terminación rose se llama **"rose"**, nunca "cobre".
- El tratamiento de color es **"PVD"**, nunca "PDB".
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

## 5. Estado de cada sección

1. **Nav** sticky: Concepto · Modelos · Compra segura · Contacto (`scroll-margin-top:88px`).
2. **Hero**: foto `essentia-hero-trio.png` + wordmark oficial (máscara CSS). **Video encima**
   que arranca **6 s después del primer movimiento de mouse** (o touch/scroll/teclado),
   **desde el segundo 4.4** (en 4.0 el video está en fundido a negro), en loop que vuelve a
   4.4. Constantes `ESPERA` y `DESDE` en el script del final.
   Mobile: banner vertical propio `essentia-hero-mobile-trio.png`.
3. **"Diseño puro"**: video `video-essentia.mp4` (6,3 MB, comprimido con ffmpeg desde 70 MB).
4. **"La familia Essentia completa"**: banner `familia-essentia-3.jpg` con el título adentro.
5. **"Conocé cada detalle"**: dos tabs.
   - *Modelos y colores*: pills + swatches por modelo (640/670/671 = satinado+negro; 660 suma
     rose) + medida de **cuba** debajo. Ciclo automático que recorre cada modelo por todos sus
     colores, arrancando en la **660 rose**.
   - *Instalación*: **esquema SVG en corte dibujado por código** (mesada tramada vs pileta de
     acero) con los 3 modos + link "Ver tutorial" que **cambia de video de YouTube según el modo**.
6. **Accesorios**: 4 cards (barra, tabla, cesto, rejilla), fotos recortadas al contenido.
   **Cada card es un link a la tienda** con hover (se eleva + "Ver en la tienda →").
   Cesto y rejilla **alternan satinado↔color** (a la vez, cada 2,8 s).
7. **"Essentia desde adentro"**: planos técnicos en acordeón.
8. **"Dónde comprar"**: mapa con distribuidores **DEMO**.
9. **CTA "Compra segura"** + garantía de 10 años + nota de Mercado Libre (texto, no botón).
10. **Newsletter**: 4 perfiles en pills (Para mi casa · Arquitectura · Mueblería · Instalación).
    En GitHub solo valida y agradece; en Shopify se convierte en alta de cliente y **el perfil
    viaja como etiqueta** para segmentar.
11. **Footer** + botones flotantes: catálogo (Google Drive) y WhatsApp (+54 9 11 2856-3001),
    ambos outline; el de catálogo queda **clavado al desagüe** de la foto del hero.

## 6. Pendientes abiertos

**Esperando respuesta del cliente**
- [ ] **Cuál de las 4 barras escurridoras** de la tienda es la de la foto (compacta 50, 50 Black,
      60, 60 Black). Hoy esa card va a la colección de accesorios.
- [ ] **La rejilla flexible no existe como producto** en la tienda → su card también va a la
      colección. Si la dan de alta, enlazarla.
- [ ] ¿Reponer el **dosificador**? Se sacó porque no estaba entre las fotos nuevas.
- [ ] **Publicar la página de Shopify** (hoy oculta). Al publicarla: verificar en vivo y hacer
      una **suscripción de prueba** al newsletter para confirmar el alta con etiqueta.

**Técnicos**
- [ ] **Hero en alta resolución** (el actual es 1672px, se ablanda en 2K/4K). Ideal ~3344x1630,
      JPG/WebP, piletas abajo y tercio superior negro.
- [ ] **Distribuidores reales** del mapa (hoy son de ejemplo: Palermo, Belgrano, Caballito, Recoleta).
- [ ] **Barra escurridora en negro** con el MISMO encuadre que la satinada, para que también alterne.
- [ ] Link real de **Mercado Libre** (hoy es una nota de texto porque no hay a dónde mandar).

## 7. Herramientas útiles ya instaladas

- **ffmpeg**: `npm i ffmpeg-static` en el scratchpad → binario en `node_modules/ffmpeg-static/ffmpeg.exe`.
  Receta de compresión usada: `-an -crf 27 -preset medium -movflags +faststart`.
- **@napi-rs/canvas** en el scratchpad, para medir/recortar/normalizar imágenes por código.
