# 0008. Código MIT, assets CC BY 4.0 y créditos 1 a 1

- **Estado:** Aceptada
- **Fecha:** 2026-11-16

## Contexto

El repo es público. El código y los assets necesitan licencias distintas, y hay que saber el origen de cada asset.

## Opciones

### Código

- **MIT.** Simple, la misma de Godot. En contra: permite usar el código en productos cerrados.
- **Apache-2.0.** En contra: más texto y obligaciones; su cláusula de patentes no importa aquí.
- **GPL-3.0.** En contra: espanta a quien solo quiere copiar un patrón.

### Assets propios

- **CC BY-NC 4.0.** En contra: no es "open" en sentido estricto.
- **CC BY 4.0.** En contra: alguien puede vender los modelos o usar a los personajes, con crédito.
- **Todos los derechos reservados.** En contra: un clon del repo no se puede redistribuir completo.

### Cómo se lleva el origen de cada asset

- **REUSE** (FSFE). En contra: no guarda la URL de origen y suma una herramienta.
- **Una tabla en `CREDITS.md` más un script.**

## Decisión

- Frontera: los assets son todo lo que está bajo `assets/` (modelos, texturas, audio, fuentes, textos de la historia y diálogos). El código es todo lo demás (`.cs`, `.tscn`, `.tres`, datos de trucos).
- Código: MIT. Assets propios: CC BY 4.0.
- Assets de terceros: solo CC0 o CC BY 4.0. OFL solo para fuentes tipográficas, porque casi todas las fuentes libres usan esa licencia.
- `CREDITS.md` lleva una fila por archivo de `assets/`, incluidos los propios y los CC0: archivo, autor, fuente (URL), licencia y cambios. Los `.import` de Godot no cuentan.
- Se acepta que alguien pueda vender los modelos o usar a los personajes dando crédito.

No es asesoría legal.

## Cómo se verifica

Un script, en el hook de pre-commit y en `just verify`, falla si:

- un archivo de `assets/` no tiene fila;
- una fila apunta a un archivo que no existe;
- la licencia no es CC0-1.0, CC-BY-4.0, propia (CC BY 4.0), u OFL-1.1 fuera de una fuente tipográfica.

## Fuentes

- https://choosealicense.com/licenses/ (consultada el 2026-11-16)
- https://creativecommons.org/licenses/ (consultada el 2026-11-16)
