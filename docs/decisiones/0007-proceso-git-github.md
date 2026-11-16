# 0007. Squash merge a main protegido, con el rastro de la IA en los commits

- **Estado:** Aceptada. La parte "Sin tablero de Projects" fue reemplazada por 0011.
- **Fecha:** 2026-11-16

## Contexto

El repo es público y de una persona. El historial y los issues tienen que verse ordenados. Ya estaba decidido: nunca directo en `main`; Conventional Commits con el tipo en inglés y la descripción en español; el push al final; ver el PR antes de crearlo; e integrar `main` a la rama con merge cuando hay una revisión abierta.

## Opciones

### Cómo entra un PR a main

- **A. Squash.** Un commit por PR, con el título del PR. En contra: el detalle de los commits de la rama queda solo en el PR.
- **B. Merge commit.** En contra: `main` se llena de commits chicos y de merges.

### Rastro de la IA

- **A. Trailer `Co-Authored-By`.** En contra: queda en cada commit para siempre.
- **B. Sin rastro.** En contra: el historial no muestra cómo se hizo el trabajo.

## Decisión

- Squash, y el trailer `Co-Authored-By`.
- Rama principal `main`. Ramas `tipo/número-descripción`.
- Un issue es un PR, revisable de una sentada.
- Issues con formularios (`feature`, `bug`, `tarea`) y etiquetas de tipo y de capa (`domain`, `application`, `presentation`, `assets`). Sin tablero de Projects.
- PR: el título en formato Conventional Commit. El cuerpo lleva Qué, Por qué, Cómo se verificó, Prueba manual y `Closes #N`.
- Prueba manual: el usuario recorre lo nuevo jugando y escribe qué recorrió y cómo se sintió. La IA no toma capturas; su evidencia es el comando de verificación y su salida.
- El merge lo hace el usuario, con 0 aprobaciones requeridas (GitHub no permite aprobar el propio PR). Lo que protege `main` son los checks.
- Versiones: SemVer 0.x, con un tag `vX.Y.Z` por hito jugable. Las builds van en GitHub Releases, con las notas generadas por GitHub.

## Cómo se verifica

- El ruleset de `main` (ver 0009) exige PR, CI verde e historial lineal.
- La CI valida el título del PR con commitlint.
- El hook `commit-msg` valida cada commit con commitlint.

## Fuentes

- Conversación de arranque con el usuario, 2026-11-16.
