# TV del local — Rice to Meet You

Bucle de 28 escenas para la pantalla del local. 1920×1080, se escala solo a
la pantalla que tenga la TV y gira en bucle infinito (~5:06 a velocidad 1).

Todo está en `index.html`: CSS y JS incrustados, sin dependencias ni build.
Lo único que carga de fuera son las tipografías (Anton, Poppins y Space Mono,
desde Google Fonts) y lo que hay en `assets/`.

## Versión 2 (noviembre de 2026)

El contenido es el mismo que en la versión 1: mismas 28 escenas y mismos
textos. Cambia cómo se mueve:

- **Más pausado.** Cada escena dura 1,5 s más (el bucle pasa de 4:24 a
  5:06) y las entradas son más lentas y van más separadas, para que cada
  movimiento se desarrolle entero.

- **Las escenas se despiden.** Antes, al cambiar de escena, el texto
  desaparecía de golpe mientras el fondo fundía. Ahora el contenido se retira
  (0,52 s) y la escena siguiente funde por encima (1,2 s).
- **Las entradas frenan al llegar.** Todas usaban la misma curva simétrica,
  que arranca lenta. Ahora hay tres curvas: una para entrar, otra para lo que
  cambia en pantalla y otra para salir.
- **Titulares y lockups de camiseta** suben dentro de una máscara fija y solo
  mueven `transform`. Antes animaban `clip-path` y `letter-spacing`, que
  obligan al navegador a recalcular en cada fotograma.
- **Aros e ilustraciones** siguen moviéndose, muy despacio, toda la escena.
  La ilustración ya no se apaga sola antes de que la escena termine.
- **Ocho escenas burdeos llevan vídeo real del local de fondo**, bajo un velo
  burdeos ligero (ver más abajo).
- **Contador de reseñas:** arranca cuando la cifra ya se ve y, si se sube la
  velocidad del bucle, se acorta (antes se alargaba). Las cinco estrellas
  entran una a una.
- **Carta de tapas:** dos filetes marcan la ventana por la que pasa la lista.
- **Orden de entrada** corregido en las escenas 14 y 18, donde la última
  línea entraba antes que el último dato de la fila anterior.

La versión 1 sigue en el historial de git, por si hay que volver a ella.

## Puesta en marcha

Abrir `index.html` en el navegador de la TV a pantalla completa. Nada más.
Si la TV no tiene navegador, sirve la carpeta desde cualquier sitio estático
(GitHub Pages vale) y apunta el reproductor a esa URL.

Si se corta la luz o se refresca la página, el bucle **retoma donde se quedó**:
guarda la posición en `localStorage`.

## Ajustes por URL

| Parámetro | Qué hace | Por defecto |
|---|---|---|
| `?speed=1.35` | Velocidad global, de 0,4 a 3. A 1,35 el bucle dura ~3:47 | `1` |
| `?band=0` | Oculta la banda inferior | banda visible |
| `?reviews=512` | Fija la cifra de reseñas a mano | sale de `reviews.json` |
| `?video=0` | Desactiva los vídeos de fondo; las escenas quedan en burdeos sólido | vídeos activos |
| `?scene=16` | Arranca en esa escena (1–28). Para revisar | donde se quedó |
| `?hold=1` | Se queda en la escena sin avanzar. Para revisar | avanza |

Se pueden combinar: `index.html?speed=1.35&band=0`

Si la TV va justa y los vídeos se ven a saltos, `?video=0` deja el bucle
como estaba, con todas las demás mejoras.

## Qué hay dentro

- `index.html` — el bucle entero.
- `assets/` — las ilustraciones de ingrediente, el sello de marca y el QR.
- `assets/video/` — los ocho clips de fondo (unos 40 MB en total).
- `reviews.json` — la cifra de reseñas. **No hace falta tocarlo a mano.**

## Los vídeos de fondo

Solo van en escenas burdeos. El vídeo queda debajo de un velo burdeos sólido
al 46 %, que arranca opaco y se abre cuando el vídeo ya está reproduciéndose.
En estas escenas el texto espera 1,3 s antes de entrar, para que primero se
vea el vídeo, y el blanco secundario sube del 65 % al 80 % para que se siga
leyendo sobre la imagen.
Si un vídeo no carga o la TV no lo reproduce, el velo no se abre y la escena
se ve en burdeos sólido, como en la versión 1.

| Escena | Clip | Original | Tramo |
|---|---|---|---|
| 5 · Arroz Albufera | `arroz-hirviendo.mp4` | `TEAM B. RTMY/C2929.MP4` | 0,4–7,0 s |
| 7 · Hay paella para todos | `caldo.mp4` | `RTMU JOR LEO/VID/C1295.MP4` | entero |
| 9 · No hace falta saber cocinar | `sofrito-manos.mp4` | `RTMU JOR LEO/VID/C1290.MP4` | 0,3–6,8 s |
| 12 · Dos horas y media | `taller-monitor.mp4` | `RTMU JOR LEO/VID/C1282.MP4` | 8,0–14,5 s |
| 16 · Reserva (QR) | `taller-neon.mp4` | `Talleres/C3275.MP4` | 0,2–6,8 s |
| 20 · Tres formatos | `equipo-sala.mp4` | `TEAM B. RTMY/C2937.MP4` | 10,5–17,3 s |
| 24 · Lo mejor no es el plato | `paella-fuego.mp4` | `TEAM B. RTMY/C2933.MP4` | entero |
| 26 · Restaurante 13–23 | `mesa-paella.mp4` | `TEAM B. RTMY/C2937.MP4` | 0–5,6 s |

Los originales están en el SSD (`RTMY SSD/RICE TO MEET YOU/`). Cada clip va a
cámara lenta (los originales son a 60 fps: se pasan a mitad de velocidad, y
`caldo.mp4` a 0,4×), en 1920×1080, H.264, sin audio, algo oscurecido y con
las luces altas recortadas para que el texto se lea encima. Duran entre 11 y
13,6 s. Si un clip dura menos que su escena, el script lo reproduce un poco
más despacio (como mínimo a 0,6×) para que no llegue a repetirse a la vista.

Para **cambiar o añadir** un vídeo: la escena lleva `data-video="assets/video/…"`
en su `<div class="sc bur …">`. Basta poner o quitar ese atributo; el script
monta el vídeo y el velo. Solo en escenas `bur`.

Para **recodificar** un tramo con el mismo tratamiento (ffmpeg):

```
ffmpeg -ss 0.4 -t 6.6 -i ORIGINAL.MP4 \
  -vf "setpts=2*PTS,scale=1920:1080,eq=brightness=-0.05:saturation=0.9,curves=all='0/0 0.6/0.5 1/0.72'" \
  -r 30000/1001 -an -c:v libx264 -preset fast -crf 25 -maxrate 4M -bufsize 8M \
  -pix_fmt yuv420p -profile:v high -level 4.1 -movflags +faststart SALIDA.mp4
```

Las personas que salen en los clips han dado permiso para aparecer en la
pantalla del local.

## La cifra de reseñas se actualiza sola

Cada noche, a las 05:00 UTC, la acción `.github/workflows/actualizar-resenas.yml`
le pide la cifra al mismo Apps Script que alimenta el contador de
`ricetomeetyou.fun`, y la escribe en `reviews.json`. Como la TV carga esta
página desde GitHub Pages cada día, se encuentra el número ya hecho: **la
pantalla no llama nunca a Google**, así que no hereda las esperas ni los
fallos pasajeros de ese endpoint.

Si el endpoint no responde o devuelve algo raro, la acción **no toca nada** y
se queda la última cifra buena. Si además faltara `reviews.json`, el
`index.html` lleva un número de respaldo escrito dentro.

Dos cosas de las que depende, por si algún día deja de actualizarse:

- En **Settings → Actions → General → Workflow permissions** el repo tiene que
  estar en *Read and write permissions*; si no, la acción no puede guardar el
  cambio.
- GitHub **desactiva las acciones programadas tras 60 días sin actividad** en
  el repo. Si pasa, sale un aviso y se reactivan con un clic.

Para forzarla a mano: pestaña **Actions → Actualizar reseñas → Run workflow**.

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
- Sin degradados, sin blur, sin glassmorphism. El velo de los vídeos es un
  color sólido con transparencia, no un degradado.
- Movimiento cálido y pausado: solo fades, fade-up, derivas cortas y escala
  de 0,92 a 1. Nada de rebotes, elásticos, flashes, wipes ni giros.
- Cuerpo nunca por debajo de 34 px: se lee de lejos y sin sonido.
- Nunca se escribe "paellera" para el recipiente. Sin emojis.

Las ilustraciones `aceite`, `ajo`, `calamar`, `cebolla` y `azafran` llevan el
canto blanco de pegatina pegado al borde del PNG, así que **solo se usan sobre
crema**, donde no se nota. `vitroceramica` queda fuera del bucle: no es un
recorte, es una placa clara a sangre.
