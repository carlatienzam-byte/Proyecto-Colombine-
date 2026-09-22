# Carmen de Burgos · Realidad Aumentada con marcador

Experiencia de RA para el IES Carmen de Burgos Seguí: al apuntar con el móvil o la tablet al logotipo del instituto aparece el busto 3D de Carmen de Burgos sobre él. Hecha con **A-Frame 1.5.0** y **MindAR 1.2.5** (seguimiento de imagen), sin apps: funciona en el navegador.

## Contenido

```
ar-carmen-de-burgos/
├── index.html               ← la experiencia completa (HTML + CSS + JS)
├── marcador-imprimir.pdf    ← el logotipo listo para imprimir en A4
└── assets/
    ├── targets.mind         ← el logotipo compilado como marcador de MindAR
    ├── carmen-burgos.glb    ← el modelo optimizado (Draco)
    ├── logo.png             ← el logotipo para la interfaz (pantalla de inicio y visor)
    └── draco/               ← decodificador Draco en local (no depende de CDN de Google)
```

## Publicarlo en GitHub Pages

1. Sube la carpeta `ar-carmen-de-burgos` entera al repositorio (por ejemplo, dentro de `Proyecto-Colombine-`).
2. La dirección quedará como `https://carlatienzam-byte.github.io/Proyecto-Colombine-/ar-carmen-de-burgos/`.
3. Ábrela desde el móvil. **Tiene que ser https**: la cámara no funciona desde `http://` ni abriendo el archivo con doble clic. Para probar en local, vale `http://localhost` en el propio ordenador.

## Cómo se usa

- Pantalla de inicio → **Comenzar** → aceptar el permiso de cámara → apuntar al logotipo.
- **Un dedo**: girar el busto. **Dos dedos**: cambiar su tamaño (y girar).
- Barra inferior:
  - **Mesa / Pared**: *Mesa* cuando el logotipo está tumbado sobre una superficie (el busto se levanta del papel, con sombra suave). *Pared* cuando está en vertical: un cartel, la pizarra digital o la pantalla de otro dispositivo (el busto sale hacia ti). Se recuerda la elección.
  - **Girar**: rotación automática.
  - **Foto**: combina la imagen de la cámara con el modelo 3D; se puede guardar o compartir.
  - **Reiniciar**: vuelve al tamaño y la orientación originales.
- Botón **i**: una breve ficha sobre Carmen de Burgos.

## Consejos para el marcador

- Imprime `marcador-imprimir.pdf` en papel **mate**, al 100 %; un mínimo de unos 10 cm de alto para el logotipo funciona bien.
- Evita brillos, arrugas y sombras fuertes sobre el papel. También funciona mostrando el logotipo en la pantalla de otro dispositivo o en un cartel.
- Vale también el JPG original a A4 con márgenes blancos: el marcador se ha compilado solo con el logotipo recortado, así que el resto del folio no afecta.

## Optimización del modelo

| | Original | Optimizado |
|---|---|---|
| Triángulos | 555.212 | 99.932 |
| Tamaño | 16,6 MB | 0,73 MB |

Pasos con glTF-Transform: `dedup → prune → weld → simplify (ratio 0,18, error 0,0005) → draco`. Las texturas (4 × 1024 px JPEG) se mantienen tal cual, sin WebP. Se ha comparado el render del original y del optimizado desde 4 ángulos y no hay diferencias visibles.

## Ajustes rápidos (en `index.html`)

- **Tamaño del busto**: `scale="0.45 0.45 0.45"` de `#carmen` (el ancho del logotipo es 1 unidad). Si cambias la escala, ajusta `position="0 0.428 0"` a `0.95 × escala` para que la base siga tocando el papel.
- **Posición sobre el logotipo**: objeto `MODOS` del script (`pos` de *mesa* y *pared*).
- **Estabilidad frente a temblores**: en `mindar-image`, `filterMinCF` y `filterBeta` (valores más bajos = más estable pero con más retraso).
- **Límites de la pinza**: `escalaMin` / `escalaMax` del componente `gestos`.

## Si cambia el logotipo

Hay que regenerar `assets/targets.mind` con el compilador oficial de MindAR: https://hiukim.github.io/mind-ar-js-doc/tools/compile (sube la imagen, descarga `targets.mind` y sustituye el archivo).

## Compatibilidad

Android (Chrome), iPhone / iPad (Safari, iOS 15 o superior) y ordenadores con webcam. No necesita ARCore ni WebXR, así que debería ir también en tablets de gama media sin soporte de RA nativa (conviene probarlo en la Lenovo IdeaTab y el OPPO del aula).
