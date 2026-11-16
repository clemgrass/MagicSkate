# 0001. La forma del combo define la forma del ataque

- **Estado:** Aceptada
- **Fecha:** 2026-11-16

## Contexto

El puntaje de los trucos se convierte en daño mágico. Al apretar el botón de ataque, el combo acumulado sale como un ataque. Se quieren varios tipos de ataque sin asignar un hechizo a cada truco.

Los controles son estilo Tony Hawk clásico (botones, multiplicador, arcade) y no los gestos de stick de Skate 3. La razón: la mecánica necesita combos largos que se puedan contar.

## Opciones

### A. La tabla define el elemento

- A favor: es simple, se entiende al tiro y encaja con la historia.
- En contra: dentro de una pelea hay un solo tipo de ataque, salvo que se cambie de tabla.

### B. La familia de trucos que domina el combo define la forma del ataque

- A favor: la variedad sale de cómo patinas. Es "magia en movimiento" literal.
- En contra: el jugador necesita ver qué ataque está cargando, así que pide una interfaz clara.

### C. El lugar del lanzamiento define el ataque (aire, aterrizaje, rail)

- A favor: es lo más fácil de leer.
- En contra: da pocos tipos de ataque.

## Decisión

B como núcleo. A queda como una capa posterior: la tabla le pondrá el elemento a la forma que armó el combo.

Riesgo y recompensa, con la regla de Tony Hawk:

- si te golpean o te caes, pierdes el combo entero;
- bloquear con magia gasta parte del combo actual;
- esquivar con un truco a tiempo no cuesta nada y suma al combo.

Se acepta el costo de diseñar una interfaz que muestre el ataque que se está cargando.

## Cómo se verifica

Escenarios grabados en `MagicSkate.Domain.Tests`: un combo dominado por cada familia produce su forma de ataque; un golpe o una caída dejan el combo en cero; bloquear descuenta y esquivar suma.

## Fuentes

- Conversación de arranque con el usuario, 2026-11-16.
