# 0005. Versiones fijadas en global.json, Directory.Packages.props y mise.toml

- **Estado:** Aceptada
- **Fecha:** 2026-11-16

## Contexto

Si cada máquina usa otra versión, el resultado cambia. Hay que fijar .NET, Godot, los paquetes NuGet y las demás herramientas en archivos del proyecto que también use la CI.

## Opciones

### Godot en la máquina local

- **A. `godot-mono` de Arch congelado** con `IgnorePkg`. En contra: congelar paquetes en Arch puede chocar con actualizaciones parciales.
- **B. GodotEnv** (Chickensoft). En contra: es una dependencia de terceros más.
- **C. Godot vía mise.** En contra: probablemente instala la build sin C# (no verificado).
- **D. Dos instalaciones.** `godot-mono` de Arch sigue actualizándose para otros usos, y la 4.7.2 .NET oficial va en una carpeta con la versión en el nombre, apuntada por `$GODOT`.

### Edad mínima de una versión

- **Plazo automático** con el `cooldown` de Dependabot. En contra: innecesario para este proyecto, y su soporte para NuGet no está verificado.
- **Revisión a mano** al actualizar.

## Decisión

- `global.json` es la fuente única: fija el SDK de .NET exacto (banda 10.0.1xx, `rollForward: disable`) y `Godot.NET.Sdk` 4.7.2. Los `.csproj` no repiten la versión. El número exacto del SDK se elige en la primera tarea, con al menos 7 días desde su publicación.
- NuGet: `Directory.Packages.props` con versiones exactas, lock files, y la CI restaura en `--locked-mode`.
- Las demás herramientas (just, lefthook, gitleaks, commitlint, osv-scanner, markdownlint) se fijan en `mise.toml`.
- Godot local: D. `$GODOT` se define en `mise.local.toml`, que no se versiona.
- Edad mínima de 7 días: se revisa a mano. Sin plazo automático.
- Instala y actualiza el usuario.

## Cómo se verifica

- La CI usa `setup-dotnet` y `setup-godot` leyendo `global.json`, y `mise` leyendo `mise.toml`.
- `just verify` falla si `$GODOT --version` no coincide con `global.json`.
- `--locked-mode` falla si un lock file no coincide.

## Fuentes

- https://github.com/chickensoft-games/setup-godot (consultada el 2026-11-16)
- https://chickensoft.games/docs/how_csharp_works (consultada el 2026-11-16)
- https://github.com/chickensoft-games/godotenv (consultada el 2026-11-16)
- https://docs.github.com/en/code-security/reference/supply-chain-security/dependabot-options-reference (consultada el 2026-11-16)
