# 0010. Pocos controles para la IA: hooks de git y permisos de Claude Code

- **Estado:** Aceptada
- **Fecha:** 2026-11-16

## Contexto

Una regla que solo está escrita se olvida en una sesión larga, y un control que estorba se termina esquivando. Conviene elegir pocos controles y que sirvan.

## Opciones

### Archivos de control (`.claude/`, `lefthook.yml`, scripts de control)

- **A. La IA los edita pidiendo confirmación.** En contra: depende de que el usuario lea lo que aprueba.
- **B. Prohibidos para la IA.** En contra: las primeras tareas, que los crean, las tendría que hacer el usuario.

### Hooks de Claude Code (por ejemplo, correr `just check` al cerrar cada turno)

- **Sí.** En contra: suman lentitud y duplican lo que ya hace el hook de pre-commit.
- **No.**

## Decisión

- Hooks de git con lefthook, para todos:
  - `pre-commit`: formato, gitleaks, el límite de 2 MB y `CREDITS.md`, solo sobre los archivos que cambiaron;
  - `commit-msg`: commitlint;
  - `pre-push`: `just verify`.
- Permisos en `.claude/settings.json`, versionado:
  - prohibido: `git push --force`, `--no-verify`, `gh pr merge`, instalar (`dotnet add package`, `mise use`, `mise install`, `sudo`), leer `.env*`, `~/.ssh` y `~/.config/gh`;
  - con confirmación: `git push`, `gh pr create`, y editar `.claude/`, `lefthook.yml` y los scripts de control (A).
- Sin hooks de Claude Code.
- Queda como instrucción en AGENTS.md: correr `just check` antes de cerrar una tarea, y revisar `docs/decisiones/` antes de cambiar lo que tocan.

## Cómo se verifica

- `.claude/settings.json` y `lefthook.yml` existen en el repo con estas reglas.
- `lefthook install` deja los hooks activos (se revisa en `.git/hooks/`).

## Fuentes

- Conversación de arranque con el usuario, 2026-11-16.
- La sintaxis exacta de los permisos de Claude Code se verifica en su documentación vigente durante la tarea.
