# AGENTS.md

MagicSkate es un juego 3D de skate en Godot con C#. Los trucos arman un combo, y el combo se lanza como ataque mágico. Es un proyecto personal de portafolio, en un repo público, de una sola persona. El diseño del juego está en `docs/juego.md`.

## Forma de trabajar

### Ritmo

- Una tarea a la vez. Al terminarla, muestra el resultado y espera mi aprobación explícita antes de seguir.
- No te adelantes. Un plan aprobado no aprueba el código, ni el texto del PR, ni el paso siguiente.
- Si algo grande no está claro, pregunta antes. Una duda menor que no cambia lo que se construye, decídela tú y cuéntamela.
- Parte el trabajo grande en piezas chicas que se puedan revisar de una vez.

### Quién decide

- Decido yo. Tú investigas, propones con fundamentos y recomiendas una opción.
- Investiga en fuentes primarias y vigentes: la documentación de la versión fijada, changelogs y releases. Anota la URL y la fecha, y marca "(no verificado)" lo que no pudiste confirmar.
- Si hay varias formas válidas, elige la más estándar, simple y fácil de mantener, y explica por qué.
- Prioridad: simplicidad, mantenibilidad, seguridad, automatización y, al final, velocidad.
- Es un proyecto personal y sencillo: lo mínimo para no ser negligente. No agregues rigor que nadie pidió.
- No cambies lo decidido en `docs/decisiones/`. Si ves una mejora grande, detente y explícame sus beneficios, costos y desventajas.

### Entender antes de construir

- Primero se entiende qué quiero y por qué. Después, cómo.
- Antes de cambiar algo, lee el código y las decisiones que lo tocan. No supongas que el estado sigue igual que en la sesión anterior.

### Listo significa evidencia

- Algo está terminado cuando me muestras el comando de verificación y su salida. "Debería funcionar" no cuenta.
- Antes de dar una tarea por terminada, corre `just check`. Antes del push, `just verify`.
- Lo que no pudiste verificar se dice tal cual, con el motivo.

### Textos

- Español de Chile, tuteando: "puedes", "elige". Nunca voseo.
- Los términos técnicos van en inglés cuando no tienen traducción natural (`commit`, `deploy`).
- Oraciones cortas, lo importante primero, sin relleno. Vale para documentos, commits, PRs, comentarios del código, la interfaz y lo que me escribes.
- El código (nombres, tipos, proyectos) va en inglés. Los comentarios y documentos, en español.
- `README.en.md` es la traducción al inglés de `README.md`. Si cambias `README.md`, actualiza `README.en.md`.

### Herramientas

- Crea y edita archivos con las herramientas de edición, no con scripts de shell, para que vea cada diff.
- No salgas de la carpeta del proyecto ni toques otros repos.
- Lo que creas para probar queda versionado o documentado. Un comando que vive solo en un chat es un paso oculto.
- Nada del proyecto depende de una persona, de su cuenta ni de su máquina.
- Instalar y actualizar lo hago yo. Dame el comando listo con `!` adelante.
- Un paquete nuevo se propone con su porqué y yo lo apruebo. Revisa a mano que la versión tenga al menos 7 días desde su publicación.

### Continuidad

- Deja el estado al día en el issue de la tarea: qué quedó a medias y cuál es el próximo paso concreto. Hazlo antes de quedarte sin contexto.
- Las decisiones que tomo en la conversación se anotan apenas las tomo.

## Documentos

| Pregunta | Dónde |
|---|---|
| Qué es el juego y por qué | `docs/juego.md` (vivo) |
| Por qué se decidió algo | `docs/decisiones/NNNN-titulo.md` |
| Cómo se construye una feature | `docs/specs/AAAA-MM-DD-tema.md` |
| En qué pasos | `docs/planes/AAAA-MM-DD-tema.md` |
| Qué hay que hacer | GitHub Issues |

- Usa la plantilla `docs/decisiones/0000-plantilla.md`. Las alternativas son reales y llevan sus contras. Si falta una obvia, agrégala marcada "(no discutida)".
- Una decisión nace `Propuesta` y pasa a `Aceptada` cuando la confirmo. Dice cómo se verifica que el código la cumple.
- Una decisión aceptada no se reescribe: se reemplaza por una nueva, y la vieja queda `Reemplazada por NNNN`.
- Lo que no está claro queda como pregunta abierta. Nunca se rellena inventando.

## Arquitectura

Tres proyectos, nombrados como en Clean Architecture (ver `docs/decisiones/0003`):

- `MagicSkate.Domain`: las reglas en C# puro. Trucos, puntaje, combo, bail, forma del ataque, daño, decisiones de los enemigos.
- `MagicSkate.Application`: el puente, sin Godot. Recibe los hechos de cada tick y entrega estado y eventos.
- `MagicSkate.Presentation`: el proyecto Godot. Física, input y dibujo. Se mantiene delgado.

Reglas:

- Presentation depende de Application, y Application de Domain. Domain y Application nunca referencian `GodotSharp`; un test lo verifica.
- Godot calcula la física (rodar, saltar, chocar). Domain decide con los hechos que recibe: suelo, rail o aire, velocidad, altura, ángulo de aterrizaje.
- La lógica avanza en ticks fijos desde `_PhysicsProcess`. Domain es determinista: el tiempo y el azar entran inyectados.
- Los trucos son datos (nombre, puntos base, familia), no código.
- No crees el namespace `MagicSkate.Godot`: tapa al namespace `Godot` del motor.

## Stack

- Godot 4.7.2 .NET y .NET 10. `global.json` es la fuente única: fija el SDK exacto y `Godot.NET.Sdk`. Los `.csproj` no llevan la versión de `Godot.NET.Sdk`; si Godot la vuelve a agregar al guardar, descarta ese cambio.
- Paquetes NuGet en `Directory.Packages.props`, con versiones exactas y lock files. Solo nuget.org.
- Las demás herramientas se fijan en `mise.toml`.
- El Godot del proyecto se ubica con `$GODOT`, definido en `mise.local.toml` (no versionado). El chequeo falla si su versión no coincide con `global.json`.

## Comandos

- `just check`: formato, build con warnings como errores y tests. Después de cada cambio.
- `just verify`: `check` más los chequeos de assets, de `Skip`, de la versión de Godot, de markdown, de seguridad (gitleaks, osv-scanner) y el smoke de Godot. Antes del push; es lo mismo que corre la CI.
- `just export`: builds de Windows y Linux.

## Testing

- TDD: escribe el test, míralo fallar por la razón correcta y después implementa.
- Cada comportamiento nuevo lleva al menos un caso feliz, uno borde y uno negativo.
- xUnit v3 con sus propios asserts. Nada de `Skip`: un control lo rechaza.
- Dobles en este orden: real, fake, stub y, al final, mock. Solo de tipos propios y escritos a mano. Un SDK de terceros se envuelve.
- Datos sintéticos y deterministas.
- Los escenarios grabados (hechos por tick y resultado esperado) son la forma de testear combos completos.
- En Godot solo hay un smoke headless. El "cómo se siente" se prueba jugando.
- Cada bug parte con un test que lo reproduce. Un test inestable se reproduce y se arregla, sin esperas, reintentos ni `Skip`.
- La cobertura es análisis, no meta. Si un test no se puede escribir, se declara.

## Assets y licencias

- Git normal, sin LFS. Máximo 2 MB por archivo; un control lo hace cumplir. Guarda los `.blend` con compresión y separa cada nivel en varios archivos.
- Si algo no cabe, se rehace o queda fuera del repo, anotado en `docs/assets-externos.md` con su ubicación.
- Código: MIT. Assets propios: CC BY 4.0. De terceros, solo CC0 o CC BY 4.0, y OFL solo para fuentes tipográficas.
- Todo archivo de `assets/` tiene su fila en `CREDITS.md` (archivo, autor, fuente, licencia, cambios), aunque su licencia no lo exija. Un control lo verifica.

## Git y GitHub

- Nunca directo en `main`: siempre rama y PR. Rama `tipo/número-descripción`, por ejemplo `feat/12-multiplicador-combo`.
- Un issue es un PR. Todo issue nuevo se agrega al proyecto "MagicSkate" de GitHub Projects.
- Conventional Commits, con el tipo en inglés y la descripción en español, muy concisos: `feat: agregar multiplicador de combo`.
- Los commits llevan el trailer `Co-Authored-By` de la IA.
- El push va al final de la tarea, después de `just verify`.
- Muéstrame el título y el cuerpo del PR antes de crearlo. El título va en formato Conventional Commit. El cuerpo lleva: Qué, Por qué, Cómo se verificó, Prueba manual y `Closes #N`.
- La sección "Prueba manual" la escribo yo: qué recorrí jugando y cómo se sintió. Tú no tomas capturas.
- El merge lo hago yo, con squash. Con una revisión abierta, `main` se integra a la rama con merge y no con rebase.
- Releases: SemVer 0.x, con un tag `vX.Y.Z` en cada hito jugable.

## Límites

- No leas secretos ni credenciales.
- No hagas force push, no te saltes los hooks (`--no-verify`) y no hagas merge de PRs.
- No instales ni actualices herramientas ni paquetes.
- No cambies la configuración de tus controles (`.claude/`, `lefthook.yml`, scripts de control) ni la de GitHub (rulesets, settings) sin mi confirmación.
