# 0006. TDD con xUnit v3 fuera del motor; en Godot, solo smoke

- **Estado:** Aceptada
- **Fecha:** 2026-11-16

## Contexto

TDD venía decidido. Falta elegir el runner, los niveles y qué se testea dentro de Godot.

## Opciones

### Runner

- **xUnit v3 (serie 4.0).** Es el más usado en .NET moderno y corre por defecto sobre Microsoft Testing Platform v2.
- **NUnit / MSTest.** En contra: no aportan nada aquí y tienen menos ejemplos.

### Tests dentro de Godot

- **A. Solo un smoke headless.** En contra: un bug en cómo Presentation lee la física solo se ve en la prueba manual.
- **B. gdUnit4Net desde el inicio.** En contra: su versión para Godot 4.7 (5.1) sigue en *release candidate*, y suma configuración y tiempo de CI.

### Assertions y dobles

- **FluentAssertions.** En contra: licencia comercial desde la v8.
- **Una librería de mocks.** En contra: con la lógica determinista, los fakes alcanzan.

## Decisión

- xUnit v3 con sus propios asserts, sobre Microsoft Testing Platform, activado en `global.json`. La versión exacta se fija al instalar (las fuentes no coinciden entre 4.0.2 y 4.0.3).
- Dobles escritos a mano, solo de tipos propios.
- Niveles:
  - unit, en `Domain.Tests` y `Application.Tests`;
  - escenarios grabados (hechos por tick como datos, más el resultado esperado), también en el chequeo rápido;
  - un smoke de Godot headless (abre la escena principal, corre unos segundos, termina sin errores), en el chequeo completo y en la CI;
  - prueba manual para el "cómo se siente".
- Tests en Godot: A. gdUnit4Net se reevalúa cuando haya versión estable y una necesidad concreta.
- Zona crítica: Domain. La cobertura se mide como análisis, sin meta.

## Cómo se verifica

- `just check` corre los tests. `just verify` agrega el smoke.
- Un control rechaza cualquier `Skip` en el código de tests.

## Fuentes

- https://xunit.net/releases/v3/4.0.0 (consultada el 2026-11-16)
- https://xunit.net/docs/getting-started/v3/getting-started (consultada el 2026-11-16)
- https://github.com/godot-gdunit-labs/gdUnit4Net (consultada el 2026-11-16)
- https://github.com/godot-gdunit-labs/gdUnit4Net/issues/522 (consultada el 2026-11-16)
