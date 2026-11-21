# Pointer Cat

Como pointerpointer.com, pero con gatos: mueves el cursor y aparece una foto real de un gato apuntándote con la pata o la nariz, justo en el punto donde está el cursor. Es un sitio estático (solo archivos), sin servidor propio ni claves.

## Antes de abrirlo: extrae el zip

No abras los archivos desde dentro del zip. Windows los abre desde una carpeta temporal que se borra y el navegador muestra `ERR_FILE_NOT_FOUND`. Haz clic derecho en `pointer-cat.zip`, elige **Extraer todo…**, y abre `index.html` o `marcar.html` desde la carpeta que se crea.

## Cómo funciona

Una foto no dice hacia dónde apunta el gato, así que cada foto lleva guardado un punto: la punta de la pata o de la nariz. La página coloca la foto de modo que ese punto quede exactamente bajo el cursor, y también la usa en espejo, así que una foto sirve para apuntar hacia los dos lados. Con unas 30 fotos ya funciona bien; con 100 o más se nota mucho mejor.

Archivos:

- `index.html`: la página principal.
- `marcar.html`: busca fotos y guarda ese punto de cada una, de forma automática o a mano.
- `fotos/fotos.js`: las fotos marcadas que usa la página publicada (empieza vacío).
- `v1-cara-reemplazada.html`: la primera versión (gato dibujado con cara de foto), por si la quieres conservar.

## Paso 1: marcar las fotos (automático)

1. Abre `marcar.html`. Aparecen fotos de gatos de Openverse, un buscador abierto de fotos con licencia libre (Flickr, Wikimedia Commons y otros sitios).
2. Pulsa **Empezar marcado automático**. La página busca el gato en cada foto, detecta su pata y su nariz con un modelo de IA que corre en tu navegador, y guarda el punto. Si ve una pata estirada lejos del cuerpo, usa esa; si no, usa la nariz. Las fotos donde no ve un gato, o ninguna pata ni nariz (por ejemplo las que no son gatos), se descartan solas.
3. La primera vez descarga el modelo (unos 155 MB, desde Hugging Face) y puede tardar unos minutos según tu conexión. Después analiza una foto cada pocos segundos.
4. Al terminar, revisa la cuadrícula "Revisar marcas": el punto rosa es adonde apunta el gato. Es una detección automática y se equivoca a veces. Quita las malas con la ✕ o haz clic en la foto para mover el punto.
5. También puedes marcar a mano con un clic sobre la punta de la pata o la nariz (`S` salta, `Z` deshace), o agregar tus propias fotos con "Agregar mis fotos".

Importante: el marcado automático funciona mejor con la carpeta publicada en internet (ver abajo), porque algunos navegadores limitan el modelo en archivos abiertos desde el disco. Si desde el disco da error, la página lo avisa y puedes marcar a mano.

Tus marcas se guardan en tu navegador y "Probar la página" ya las usa. Para que valgan en la página publicada para todos:

1. Pulsa **Descargar fotos.js**.
2. Reemplaza `fotos/fotos.js` con ese archivo.
3. Vuelve a publicar la carpeta.

## Publicarla gratis

No hace falta instalar nada. Cualquiera de estas opciones sirve, porque es solo una carpeta de archivos:

- **Netlify Drop**: entra a app.netlify.com/drop y arrastra la carpeta completa. Te da una dirección pública. Crea una cuenta gratis si quieres que el sitio no se borre.
- **GitHub Pages**: sube la carpeta a un repositorio, ve a Settings → Pages y elige la rama principal.
- **Cloudflare Pages**: crea un proyecto nuevo y sube la carpeta con "Direct Upload".

## Formato de `fotos/fotos.js`

```js
window.PC_FOTOS = [
  { "url": "https://api.openverse.org/v1/images/…/thumb/", "x": 0.31, "y": 0.18, "credito": "Foto: Autor · CC BY 2.0", "enlace": "https://www.flickr.com/photos/…" },
  { "archivo": "miso.jpg", "x": 0.82, "y": 0.45, "credito": "Foto mía" }
];
```

`x` e `y` son el punto de la punta, de 0 a 1 desde la esquina superior izquierda de la foto. `url` es una foto en internet; `archivo` es una foto dentro de `fotos/`. `enlace` es opcional. El crédito se muestra abajo en la página.

## Sobre las fotos

- Las de Openverse tienen licencia libre (Creative Commons o dominio público), y la página muestra el autor y la licencia con enlace a la foto original. Algunas licencias exigen dejar el crédito; no lo quites.
- Las fotos de Reddit y de otros sitios pertenecen a quienes las publicaron. Si las pones en una página pública, pide permiso o usa solo las tuyas, y deja el crédito. Reddit ya no permite leer sus listados desde una página, por eso no se traen en vivo.
- Las fotos de Openverse se cargan desde sus servidores. Si algún día desaparece una, esa foto simplemente deja de usarse.
