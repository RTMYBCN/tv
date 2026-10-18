# TV del local — Rice to Meet You

Bucle de 26 escenas para la pantalla del local. 1920×1080, se escala solo a
la pantalla que tenga la TV y gira en bucle infinito (~4:06 a velocidad 1).

Todo está en `index.html`: CSS y JS incrustados, sin dependencias ni build.
Lo único que carga de fuera son las tipografías (Anton, Poppins y Space Mono,
desde Google Fonts) y lo que hay en `assets/`.

## Puesta en marcha

Abrir `index.html` en el navegador de la TV a pantalla completa. Nada más.
Si la TV no tiene navegador, sirve la carpeta desde cualquier sitio estático
(GitHub Pages vale) y apunta el reproductor a esa URL.

Si se corta la luz o se refresca la página, el bucle **retoma donde se quedó**:
guarda la posición en `localStorage`.

## Ajustes por URL

| Parámetro | Qué hace | Por defecto |
|---|---|---|
| `?speed=1.35` | Velocidad global, de 0,4 a 3. A 1,35 el bucle dura ~3:02 | `1` |
| `?band=0` | Oculta la banda inferior | banda visible |
| `?reviews=500` | Cifra del conteo de reseñas de Google | `500` |

Se pueden combinar: `index.html?speed=1.35&band=0`

## Qué hay dentro

- `index.html` — el bucle entero.
- `assets/` — las ilustraciones de ingrediente, el sello de marca y el QR.

`taller-paella.html` y `tv.html` son otras páginas que ya estaban en el repo
y no tienen que ver con este bucle.

El **QR** (`assets/qr-reserva.png`) apunta a
`https://www.ricetomeetyou.fun/esp/taller-de-paella-barcelona#rtmy-reservar`,
con corrección de errores nivel H para que aguante reflejos y distancia.
Si cambia esa URL, hay que regenerar el PNG.

## Reglas de marca que respeta

Sale del design system de RTMY, así que conviene no romperlas al editar:

- Burdeos `#741C1C` es el único color de marca y domina. Los fondos alternan
  burdeos sólido y crema. **Nada de negro, casi negro ni blanco puro de fondo.**
- Sobre burdeos, texto blanco o blanco al 65 %. Sobre crema, burdeos o tinta.
  Nunca tinta oscura sobre burdeos ni burdeos sobre negro.
- Sin degradados, sin blur, sin glassmorphism.
- Movimiento cálido y pausado: solo fades, fade-up, derivas cortas y escala
  de 0,92 a 1. Nada de rebotes, elásticos, flashes, wipes ni giros.
- Cuerpo nunca por debajo de 34 px: se lee de lejos y sin sonido.
- Nunca se escribe "paellera" para el recipiente. Sin emojis.

Las ilustraciones `aceite`, `ajo`, `calamar`, `cebolla` y `azafran` llevan el
canto blanco de pegatina pegado al borde del PNG, así que **solo se usan sobre
crema**, donde no se nota. `vitroceramica` queda fuera del bucle: no es un
recorte, es una placa clara a sangre.
