@AGENTS.md

## Claude Code

- Los permisos están en `.claude/settings.json`, que se versiona. Prohíben el force push, `--no-verify`, `gh pr merge`, instalar y leer credenciales. Piden confirmación para `git push`, `gh pr create` y para editar los archivos de control.
- Las skills de superpowers guardan specs y planes en `docs/specs/` y `docs/planes/`, no en `docs/superpowers/`. Ninguna skill reemplaza el ritmo de AGENTS.md: cada paso espera aprobación.

## Límites del proyecto (también para auto mode)

- Prohibido: leer secretos o credenciales; force push; `--no-verify`; hacer merge de PRs; instalar o actualizar paquetes o herramientas; push directo a `main`.
- Requiere confirmación explícita del usuario: `git push`, crear un PR, crear o editar issues, cambiar `.claude/`, `lefthook.yml`, los scripts de control o la configuración del repo en GitHub.
- Todo trabajo queda dentro de la carpeta del proyecto.
