[Español](README.md)

# MagicSkate

A 3D skateboarding game where your tricks are your magic.

In a world where everyone casts magic with carved wooden wands, the Wand Guild decides which magic is "correct". She invented another way: a skateboard, built together with her partner. When the Guild takes him away to force her to give up the board, she skates out to rescue him.

The villain isn't a beast. It's the system.

> **Status:** pre-production. The decisions are made and there is no playable code yet. Tasks live in [Issues](https://github.com/clemgrass/MagicSkate/issues).

## How it plays

- **You skate like in the classic Tony Hawk games.** Button-based tricks, long combos with a multiplier and arcade physics, in a semi-open world.
- **Your combo is your spell.** When you press the attack button, your whole accumulated score is released as a magic attack.
- **How you skate decides what you cast.** The trick family that dominates the combo sets the shape of the attack: grinds for a beam, airs for an explosion, manuals for projectiles.
- **You risk everything.** If you get hit or you bail, you lose the whole combo. Blocking with magic costs part of the combo. Dodging with a well-timed trick costs nothing and adds to it.

The full design is in [`docs/juego.md`](docs/juego.md) (in Spanish).

## How it's built

| | |
|---|---|
| Engine | Godot 4.7.2 .NET |
| Language | C# on .NET 10 |
| Style | Stylized / modern low poly |
| Platforms | Windows and Linux |

The game logic doesn't know about Godot, and Godot doesn't know about the logic:

```text
MagicSkate.Presentation  →  MagicSkate.Application  →  MagicSkate.Domain
  (Godot: physics,            (bridge: per-tick facts,    (rules in plain C#:
   input and drawing)          state and events)           tricks, combo, damage)
```

Godot computes the physics. On every tick, Domain receives the facts (on the ground, on a rail or in the air, speed, landing angle) and decides which trick it was, how much it's worth and which attack comes out. That's why the rules are written with TDD and tested with `dotnet test`, without opening the engine.

## Documentation

All documentation is in Spanish.

- [`docs/juego.md`](docs/juego.md): what the game is and why.
- [`docs/decisiones/`](docs/decisiones/): every architecture decision, with its alternatives and downsides.
- [`AGENTS.md`](AGENTS.md): how work is done in this repo, for people and AI agents.

## Licenses

- **Code:** MIT.
- **Original assets** (everything under `assets/`): CC BY 4.0.
- **Third-party assets:** only CC0 or CC BY 4.0 (and OFL for typefaces). Each file has its author and source in `CREDITS.md`.

The license files are added in one of the first tasks. Details in [`docs/decisiones/0008-licencias.md`](docs/decisiones/0008-licencias.md).

itch.io page: https://cgrassdev.itch.io/magic-skate
