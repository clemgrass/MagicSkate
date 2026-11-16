# 0011. Tablero de GitHub Projects para ver y mover las tareas

- **Estado:** Aceptada
- **Fecha:** 2026-11-16
- **Reemplaza:** la parte "Sin tablero de Projects" de la 0007.

## Contexto

La 0007 dejó los issues como una lista, sin tablero. El usuario quiere ver las tareas como tabla y moverlas entre estados, al estilo kanban, dentro de GitHub.

## Opciones

### A. Solo la lista de issues (lo que decía la 0007)

- A favor: nada que configurar.
- En contra: no muestra en qué estado está cada tarea ni permite arrastrarlas.

### B. GitHub Projects ligado al repo

- A favor: es nativo de GitHub, gratis, con vista de tabla y de board. Trae automatizaciones por defecto: un issue cerrado o un PR integrado pasan a *Done*.
- En contra: es un lugar más que mantener al día, y `gh` necesita el permiso `project`.

### C. Una herramienta externa, como Trello o Linear (no discutida)

- En contra: queda fuera de GitHub y depende de otra cuenta.

## Decisión

B.

- Proyecto "MagicSkate", del usuario, ligado al repo.
- Vistas de tabla y de board. Columnas por defecto: *Todo*, *In Progress* y *Done*.
- Las automatizaciones por defecto quedan activas.
- Todo issue nuevo se agrega al proyecto. Mover las tarjetas lo hace el usuario o las automatizaciones.
- La vista de board se crea en la web: la API de GitHub no permite crear vistas (no verificado).

## Cómo se verifica

`gh project item-list` muestra todos los issues abiertos del repo.

## Fuentes

- https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects (consultada el 2026-11-16)
