# MagicSkate

Qué es el juego y por qué. Documento vivo: cambia cuando cambia el diseño.

## Premisa

En un mundo de magia, todos usan varitas de madera tallada. El Gremio de Varitas controla cómo se hace la magia "correcta".

La protagonista, una mujer, inventa una tabla de skate y hace magia en movimiento. Su pareja, un hombre trans, la ayudó a construirla. El Gremio se lo lleva para obligarla a renunciar a la tabla, y ella va a rescatarlo.

## Pilares

- **Magia en movimiento.** La forma en que patinas es el hechizo. No hay un menú de hechizos.
- **El villano es el sistema, no una bestia.** Los enemigos son el Gremio y sus guardias, que imponen una sola forma correcta de hacer magia.
- **Combos estilo Tony Hawk clásico.** Trucos con botones, combos largos con multiplicador, física arcade.

## Mecánicas decididas

- **Mundo semiabierto.**
- **El ataque.** El combo acumula puntaje. Al apretar el botón de ataque, todo lo acumulado sale como un ataque.
- **La forma del ataque la decide el combo.** La familia de trucos que más pesa en el combo define qué sale. Nombres tentativos:
  - grinds → rayo;
  - aéreos → explosión desde arriba;
  - manuals → proyectiles.
- **Riesgo y recompensa:**
  - si te golpean o te caes, pierdes el combo entero;
  - bloquear con magia gasta parte del combo actual;
  - esquivar con un truco a tiempo no cuesta nada y suma al combo.
- **Enemigos:**
  - magos del Gremio, que lanzan distintos tipos de magia; se bloquea con magia o se esquiva con trucos;
  - guardias, con ataques cuerpo a cuerpo estándar.

Las alternativas y su porqué están en `docs/decisiones/0001-cruce-magia-skate.md`.

## Preguntas abiertas

- **Tipos de tabla.** Es una capa posterior: cada tabla le pone un elemento (fuego, hielo…) a la forma que armó el combo. Falta definir cuántas tablas hay y cómo se consiguen.
- **Familias de trucos y formas de ataque.** La lista definitiva y sus nombres.
- **Cómo ve el jugador qué ataque está cargando.** La opción elegida lo exige, y la interfaz todavía no está diseñada.
- **Estructura del mundo semiabierto.** Zonas, hub, cómo se avanza en la historia.
- **Partidas guardadas.** Formato y versionado, cuando existan.
