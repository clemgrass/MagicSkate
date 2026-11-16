# 0002. Godot 4.7.2 .NET con .NET 10, exportando a Windows y Linux

- **Estado:** Aceptada
- **Fecha:** 2026-11-16

## Contexto

El motor (Godot), el lenguaje (C#) y las plataformas (Windows y Linux) venían decididos: la última versión estable de Godot, verificada y fijada. Falta fijar las versiones y confirmar el soporte de export con C#.

## Opciones

### Versión de Godot

- **4.7.2 estable** (2026-08-18). Es la última estable y cumple 7 días desde su publicación.
- **4.8** (no discutida). Sigue en beta (beta3 al 2026-11-09). En contra: no es estable.

### Versión de .NET

- **.NET 10 LTS.** Soporte hasta 2028-11-14.
- **.NET 11.** Salió el 2026-11-10. En contra: soporte corto y no cumple 7 días.
- **.NET 8** (no discutida). Es el mínimo de Godot 4.5. En contra: su soporte termina antes.

## Decisión

- Godot 4.7.2 .NET y .NET 10. Todos los proyectos apuntan a `net10.0`.
- Export a Windows x86_64 y Linux x86_64, con las plantillas .NET. C# no exporta a web, y Android e iOS son experimentales; ninguna de esas plataformas está en el alcance.
- **(no verificado)** Que el proyecto Godot 4.7 compile con `net10.0`. Los docs de 4.7 no dicen un mínimo propio (los de 4.5 piden .NET 8 y los de 4.8 piden .NET 10). Si falla, el proyecto Godot baja a `net8.0` y el resto sigue en `net10.0`.
- **(no verificado)** Exportar a Windows desde Linux. Se prueba en la tarea de release.

## Cómo se verifica

`global.json` fija ambas versiones (ver 0005). La CI instala exactamente esas versiones y falla si el build no pasa.

## Fuentes

- https://godotengine.org/download/archive/4.7.2-stable/ (consultada el 2026-11-16)
- https://github.com/godotengine/godot/releases (consultada el 2026-11-16)
- https://docs.godotengine.org/en/4.7/tutorials/scripting/c_sharp/c_sharp_basics.html (consultada el 2026-11-16)
- https://dotnet.microsoft.com/en-us/platform/support/policy (consultada el 2026-11-16)
- https://learn.microsoft.com/en-us/lifecycle/products/microsoft-net-and-net-core (consultada el 2026-11-16)
