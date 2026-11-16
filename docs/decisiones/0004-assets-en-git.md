# 0004. Assets en git normal, sin LFS, con un máximo de 2 MB por archivo

- **Estado:** Aceptada
- **Fecha:** 2026-11-16

## Contexto

Un repo de juego carga binarios: modelos, texturas, audio. El estilo es low poly, así que los archivos deberían ser chicos.

## Opciones

### A. Git LFS desde el primer asset

- A favor: es lo estándar en juegos, y el historial queda liviano.
- En contra: hay que instalar `git-lfs`, y GitHub Free da 10 GB de almacenamiento y 10 GB de descarga al mes. Cada versión de un archivo cuenta completa, y las descargas de otros salen de esa cuota.

### B. Git normal con un límite por archivo

- A favor: nada que instalar, sin cuota. El límite empuja a aprovechar el low poly.
- En contra: cada versión de un binario queda en el historial para siempre. Pasar a LFS después obliga a reescribir el historial.

### C. Fuentes pesadas fuera del repo

- En contra: choca con la regla de que nada dependa de una persona ni de su máquina.

## Decisión

B, con un máximo de 2 MB por archivo.

- Los `.blend` se guardan con compresión, y cada nivel se separa en varios archivos.
- Si algo no cabe, se rehace o queda fuera del repo, anotado en `docs/assets-externos.md` con su ubicación. Esa es la única excepción de C.
- La música es lo que más probablemente choque con el límite: un `.ogg` de 3 minutos pesa alrededor de 2 MB.

## Cómo se verifica

Un script rechaza archivos de más de 2 MB. Corre en el hook de pre-commit y en `just verify` (la CI).

## Fuentes

- https://docs.github.com/billing/managing-billing-for-git-large-file-storage/about-billing-for-git-large-file-storage (consultada el 2026-11-16)
- https://docs.github.com/en/billing/reference/product-usage-included (consultada el 2026-11-16)
