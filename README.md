# Essentia — Landing

Landing page de la línea **Essentia** (piletas de cocina de acero inoxidable, Mi Pileta).

Sitio de una sola página (`index.html`) con todos los assets en `assets/`.
Estética dark, modelo 3D interactivo, secciones de colores/medidas/instalación, planos técnicos, accesorios y localizador de distribuidores.

## Cómo verlo
- **Online:** GitHub Pages (ver la URL en Settings → Pages).
- **Local:** abrir `index.html` en el navegador, o servir la carpeta (`npx serve`).

> El modelo 3D y las tipografías se cargan por CDN, así que necesita conexión a internet.

Ver `MINUTA-essentia.md` para el detalle de secciones, assets y pendientes.

## Por qué está el `.nojekyll`

GitHub Pages corre **Jekyll** por defecto, y Jekyll interpreta Liquid (`{%…%}`, `{{…}}`)
en los `.html` y `.md` del repo. La minuta y el handoff mencionan esas etiquetas como
texto, y eso alcanzaba para que el build fallara con "Page build failed" — el sitio se
quedaba servido en la última versión que sí compiló, sin avisar.

El archivo vacío `.nojekyll` en la raíz desactiva Jekyll y hace que Pages sirva los
archivos tal cual. **No borrarlo.**
