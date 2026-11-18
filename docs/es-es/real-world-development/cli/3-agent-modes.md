---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto, pero los cambios más amplios necesitan un proceso más sólido. GitHub Copilot CLI está diseñado para trabajar con el flujo existente de una organización y ayudar a crear lo adecuado de la manera correcta. En esta y varias lecciones siguientes seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como esperas, y finalmente integrarás el cambio en el proyecto.

> [!NOTE]
> Usarás la misma conversación y rama durante todo el flujo de trabajo de esta funcionalidad. En general, usarías ramas o PR distintas para los diferentes tipos de archivo, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación nueva de Copilot desde una incidencia de GitHub;
- definirás los requisitos en el modo Plan;
- implementarás la funcionalidad con el modo Autopilot;
- revisarás el código;
- validarás la funcionalidad manualmente en un navegador reenviado.

Mientras completas esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar detalles, como la combinación de categorías. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA a tu flujo de desarrollo no cambia los fundamentos. De hecho, los hace aún más importantes. Muchos desarrolladores siguen un proceso parecido a este:

1. Empezar por una incidencia registrada que describa el trabajo.
2. Planificar lo que se va a crear.
3. Crear y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Cuando se haya revisado el código y se hayan superado las comprobaciones de integración continua, combinar el cambio.

> [!NOTE]
> Los detalles exactos dependen del equipo y de la organización, pero la mayoría de los procesos siguen alguna variante de este modelo.

Al seguir este enfoque estándar, te aseguras de que el código generado con IA cumpla los requisitos y pase por el mismo proceso de validación que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo:

- **Interactive**: colaboras con el agente. Este sugiere cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta considerar que la tarea está terminada.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empieza por la incidencia

Antes de comenzar el trabajo de filtrado, vuelve a tu Codespace y asegúrate de que el repositorio y el terminal estén listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha combinado la PR de las valoraciones con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza por `#7` (o por un número parecido). El carácter `#` permite añadir al contexto una incidencia o una solicitud de incorporación de cambios de GitHub.

## Planifica la funcionalidad de filtrado

La planificación te permite definir cómo se implementará una funcionalidad o se realizará una tarea antes de delegarla en Copilot. Para cualquier trabajo complejo, conviene dedicar un tiempo a planificar. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe este prompt:

   ```plaintext
   Crea un plan para implementar esta funcionalidad.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque.

3. Es posible que Copilot te pregunte cómo debería funcionar el filtrado. Responde según tus preferencias; no hay una única respuesta correcta.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que modifique cualquier parte del plan que no coincida con tus decisiones.

## Autoriza Autopilot

Con el plan escrito y revisado, es hora de implementarlo. Deja que Copilot trabaje en la tarea con Autopilot.

Autopilot permite que Copilot itere hasta considerar que ha completado el trabajo.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobarlo, la implementación puede empezar de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por terminadas.

## Revisa y verifica la implementación

Como cualquier otro código, el código generado debe revisarse antes de combinarlo. Revisemos el código y ejecutemos el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones tomadas durante la planificación.
4. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista del diff.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva cualquier fallo.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda probar en el navegador la funcionalidad de filtrado. Indícame la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editorial, así como las combinaciones que acordaste en el plan.
4. Confirma que el reinicio de los filtros y el comportamiento sin resultados coinciden con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- iniciaste una conversación de Copilot desde una incidencia de GitHub;
- definiste los requisitos en el modo Plan;
- implementaste la funcionalidad con Autopilot;
- revisaste el código;
- validaste manualmente la funcionalidad en un navegador reenviado.

A continuación, [descubrirás cómo se genera el código y cómo lograr que siga las prácticas documentadas mediante instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
