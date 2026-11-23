[English](README.en.md)

# MagicSkate

Un juego 3D de skate donde tus trucos son tu magia.

En un mundo donde todos hacen magia con varitas de madera tallada, el Gremio de Varitas decide qué magia es "correcta". Ella inventó otra forma: una tabla de skate, construida junto a su pareja. Cuando el Gremio se lo lleva para obligarla a renunciar a la tabla, ella sale a rescatarlo patinando.

El villano no es una bestia. Es el sistema.

> **Estado:** preproducción. Las decisiones están tomadas y todavía no hay código jugable. Las tareas están en [Issues](https://github.com/clemgrass/MagicSkate/issues).

## Cómo se juega

- **Patinas como en los Tony Hawk clásicos.** Trucos con botones, combos largos con multiplicador y física arcade, en un mundo semiabierto.
- **Tu combo es tu hechizo.** Al apretar el botón de ataque, todo el puntaje acumulado sale como un ataque mágico.
- **Cómo patinas decide qué lanzas.** La familia de trucos que domina el combo define la forma del ataque: grinds para un rayo, aéreos para una explosión, manuals para proyectiles.
- **Arriesgas todo.** Si te golpean o te caes, pierdes el combo entero. Bloquear con magia te cuesta parte del combo. Esquivar con un truco a tiempo no te cuesta nada y además suma.

El diseño completo está en [`docs/juego.md`](docs/juego.md).

## Cómo está hecho

| | |
|---|---|
| Motor | Godot 4.7.2 .NET |
| Lenguaje | C# sobre .NET 10 |
| Estilo | Stylized / modern low poly |
| Plataformas | Windows y Linux |

La lógica del juego no conoce a Godot, y Godot no conoce la lógica:

```text
MagicSkate.Presentation  →  MagicSkate.Application  →  MagicSkate.Domain
  (Godot: física,             (puente: hechos por         (reglas en C# puro:
   input y dibujo)             tick, estado y eventos)     trucos, combo, daño)
```

Godot calcula la física. En cada tick, Domain recibe los hechos (en el suelo, en un rail o en el aire, la velocidad, el ángulo de aterrizaje) y decide qué truco fue, cuánto vale y qué ataque sale. Por eso las reglas se escriben con TDD y se testean con `dotnet test`, sin abrir el motor.

## Documentación

- [`docs/juego.md`](docs/juego.md): qué es el juego y por qué.
- [`docs/decisiones/`](docs/decisiones/): cada decisión de arquitectura, con sus alternativas y contras.
- [`AGENTS.md`](AGENTS.md): cómo se trabaja en este repo, para personas y agentes de IA.

## Licencias

- **Código:** MIT.
- **Assets propios** (todo lo que está bajo `assets/`): CC BY 4.0.
- **Assets de terceros:** solo CC0 o CC BY 4.0 (y OFL para fuentes tipográficas). Cada archivo tiene su autor y su origen en `CREDITS.md`.

Los archivos de licencia se agregan en una de las primeras tareas. Los detalles están en [`docs/decisiones/0008-licencias.md`](docs/decisiones/0008-licencias.md).

Página en itch.io: https://cgrassdev.itch.io/magic-skate
