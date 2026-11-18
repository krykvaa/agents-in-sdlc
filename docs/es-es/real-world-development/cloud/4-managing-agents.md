---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores pediste al agente de Copilot en la nube que realizara tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, sigue siendo importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas, consultar las operaciones realizadas e incluso orientar una sesión que sigue en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas;
- dirigirás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo decidió que sería buen momento para añadir también un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo controles de alternancia, parecía lógico incluirlo. Orientarás el trabajo del agente para que añada ambos modos.

## Revisa las tareas del agente de Copilot en la nube

Consulta el estado de las tareas asignadas al agente de Copilot en la nube.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en los paneles principal e izquierdo. Deberías ver las tareas asignadas a Copilot, entre ellas:
   - actualizar la documentación del código;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio.
3. Selecciona una de las tareas en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - obtener el código del repositorio;
   - preparar el entorno de trabajo de Copilot;
   - configurar servidores MCP;
   - realizar los distintos pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos exactos pueden variar según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios de la derecha. Allí puedes consultar la PR y sus archivos modificados para supervisar el trabajo.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que le haya asignado Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en el panel de control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que la sesión indique que ha terminado de prepararse y ha empezado a trabajar. Verás mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   Mientras trabajamos en el modo de alto contraste, añadamos también un modo claro. Debe haber un control para que los usuarios puedan seleccionar el modo de visualización que prefieran.
   ```

   ![Tarea del agente en la nube en la página de agentes con el cuadro de diálogo Steer active session while Copilot is working resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo reconoce y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot continuará con la tarea actualizada e incorporará la nueva solicitud después de terminar el paso en el que estaba trabajando cuando enviaste el mensaje.

El proceso tardará varios minutos. Puedes aprovechar el tiempo para repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Puedes realizar un seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar su alcance o añadir indicaciones.

Has aprendido:

- a usar el panel de control y la página de agentes para supervisar las tareas del agente en la nube;
- a redirigir una sesión en curso para solicitar otra funcionalidad.

Mientras Copilot completa las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
