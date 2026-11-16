# 0009. Un solo just verify en local y en la CI, con seguridad mínima

- **Estado:** Aceptada
- **Fecha:** 2026-11-16

## Contexto

Verde en local y rojo en la CI hace perder la confianza. El proyecto es personal y sencillo: lo mínimo para no ser negligente. No hay servidor, ni usuarios, ni datos personales.

## Opciones

### Dependabot

- **Solo alertas y PRs de seguridad.**
- **Además, PRs de actualización mensuales y agrupados.** En contra: dos PRs al mes que revisar; innecesario aquí.

### CI endurecida (SHAs fijos, zizmor, actionlint, permisos mínimos)

- **Sí.** En contra: más configuración de la que pide un proyecto personal.
- **No.** En contra: las Actions se fijan por tag de versión, que puede moverse.

### Capturas como evidencia de la IA

- **Un modo de captura de debug** o el Movie Maker de Godot. En contra: el usuario no las necesita; su objetivo es probar cada funcionalidad jugando.
- **Sin capturas.**

## Decisión

- `just` (fijado en `mise.toml`) es el punto de entrada, igual en local y en la CI:
  - `just check`: formato, build con warnings como errores, tests de Domain y Application;
  - `just verify`: `check`, más el límite de 2 MB, `CREDITS.md`, la búsqueda de `Skip`, la versión de `$GODOT`, markdownlint en `docs/`, gitleaks, osv-scanner y el smoke de Godot headless;
  - `just export`: builds de Windows y Linux.
- `ci.yml`, en cada PR y en cada push a `main`: `just verify` en Ubuntu, más el título del PR validado con commitlint.
- `release.yml`, al crear un tag `vX.Y.Z`: export, zip y GitHub Releases.
- Meta: `just verify` en menos de 3 minutos.
- Sin builds por PR, ambientes, deploy ni migraciones.
- Seguridad:
  - *secret scanning* y *push protection* en GitHub;
  - `nuget.config` con *package source mapping* (solo nuget.org);
  - Dependabot solo con alertas y PRs de seguridad;
  - sin CI endurecida: Actions por tag de versión y permisos por defecto.
- Ruleset de `main`: PR obligatorio, CI verde, historial lineal, sin force push ni borrado, sin excepciones (tampoco para el admin).
- Sin capturas de la IA.

## Cómo se verifica

- La CI corre el mismo `just verify`.
- El ruleset está activo en GitHub (se revisa en Settings → Rules).

## Fuentes

- https://docs.github.com/en/billing/concepts/product-billing/github-actions (consultada el 2026-11-16)
- https://github.com/chickensoft-games/setup-godot (consultada el 2026-11-16)
