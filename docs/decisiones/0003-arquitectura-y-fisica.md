# 0003. Tres capas: Godot mueve, Domain decide

- **Estado:** Aceptada
- **Fecha:** 2026-11-16

## Contexto

Venía decidido que el proyecto tiene tres partes: Godot (input y dibujo), la lógica en C# puro testeable sin el motor, y una capa intermedia, sin que Godot y la lógica dependan el uno del otro. Un juego de skate vive de la física, y el motor de física de Godot no se puede usar sin depender de Godot.

## Opciones

### Dónde vive el movimiento

- **A. Godot mueve, Domain decide.** Godot calcula la física. En cada tick, Application le pasa a Domain los hechos del momento.
  - A favor: usa la física que ya existe. Domain se testea con hechos inventados.
  - En contra: el "cómo se siente" patinar vive en Godot y se prueba jugando.
- **B. Física propia en Domain**, con rails como curvas y rampas como formas simples.
  - A favor: es determinista y testeable de punta a punta.
  - En contra: hay que reescribir física, mantener el nivel duplicado en dos formatos, y es mucho trabajo antes de poder jugar.

### Nombres de las capas

- **Clean Architecture:** Domain, Application, Presentation. Es reconocible en .NET y las dependencias calzan.
- **Hexagonal** (Core y Adapters). En contra: Application no es un adaptador de Godot, porque no lo conoce.
- **Nombres temáticos** (Deck, Trucks, Wheels). En contra: no son estándar.

## Decisión

A, con estos proyectos:

- `MagicSkate.Domain`: las reglas (trucos, puntaje, combo, bail, forma del ataque, daño, decisiones de los enemigos). Sin referencias.
- `MagicSkate.Application`: recibe los hechos de cada tick (suelo, rail o aire, velocidad, altura, ángulo de aterrizaje) y entrega estado y eventos. Depende de Domain. No conoce Godot.
- `MagicSkate.Presentation`: el proyecto Godot. Depende de Application.
- `MagicSkate.Domain.Tests` y `MagicSkate.Application.Tests`.

Además:

- La lógica avanza en ticks fijos desde `_PhysicsProcess`. Domain es determinista: el tiempo y el azar entran inyectados.
- Los trucos son datos (nombre, puntos base, familia).
- No se usa el namespace `MagicSkate.Godot`, porque tapa al namespace `Godot`.

## Cómo se verifica

- Las referencias entre proyectos: el compilador.
- Un test falla si `MagicSkate.Domain` o `MagicSkate.Application` referencian el ensamblado `GodotSharp`. Usa reflexión, sin librerías extra.

## Fuentes

- Conversación de arranque con el usuario, 2026-11-16.
