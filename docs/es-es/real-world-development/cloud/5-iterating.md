---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que documentara el código, creara una funcionalidad de juegos relacionados para que el equipo de diseño pudiera iterar y añadiera funciones de accesibilidad, incluidos controles para los modos de alto contraste y claro. Ahora revisarás los cambios de código y, si hace falta, pedirás a Copilot que los mejore.

### Escenario

Como hemos visto varias veces, los fundamentos del diseño de software y DevOps no cambian al incorporar IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Antes de activar la revisión para el resto del equipo, revisa las propuestas de GitHub Copilot para documentar el código, crear la funcionalidad de juegos relacionados y añadir las funciones de accesibilidad.

## Seguridad y el agente de GitHub Copilot en la nube

El agente de Copilot en la nube trabaja de forma asíncrona y sin supervisión, por lo que cuenta con restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el código.
- El agente se ejecuta en GitHub Actions, que crea un entorno independiente y temporal para su trabajo.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que pueda ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) que generó el agente de Copilot en la nube: la que añade documentación al código. La revisarás desde la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Puede que la PR muestre un aviso de que GitHub Copilot está bloqueado por un firewall. Es **lo esperado**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o desactivar el firewall del agente en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo que has aprendido.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye comentarios TSDoc y otra documentación. Los cambios exactos pueden variar. Comprueba que se hayan añadido comentarios TSDoc y encabezados de comentario, y que se sigan las convenciones de TypeScript. Estas indicaciones proceden de los archivos de instrucciones que revisaste al principio del Ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar varios minutos.

## Pide cambios a GitHub Copilot

Trabajar con Copilot en una PR no es un proceso unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios insertados en el código, igual que harías con otras personas del equipo. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como los resultados no son deterministas, no hay un texto único que debas usar. Algunas ideas:

- Añadir encabezados de comentario breves que describan cada archivo de código.
- Añadir comentarios TSDoc a los archivos TypeScript y Astro.
- Crear un README que describa la estructura de la aplicación Astro.

1. Escribe un comentario para solicitar una actualización de la documentación generada y menciona a **@copilot**. Puedes elegir una de las ideas anteriores o pedir otra mejora.
2. Selecciona **View Session** para observar cómo Copilot realiza el trabajo. Iniciará una sesión nueva para aplicar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la PR.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá una confirmación nueva en la PR.
5. Selecciona **Files changed** para revisar los cambios.

Sigue iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot generó para mostrar juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar varios minutos.
9. **Opcional:** puedes cambiar a la rama de esta PR en tu Codespace y probar manualmente la funcionalidad de juegos relacionados. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes pedirle cambios con nuevos comentarios en la pestaña **Conversation**. Por ejemplo, puedes modificar el número de juegos relacionados o pedirle que añada encabezados de comentario y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones de accesibilidad implementadas con el agente personalizado. La PR debería incluir el modo de alto contraste que asignaste en el Ejercicio 3 y el modo claro que pediste en el panel de agentes en el Ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los controles para cambiar entre los modos;
   - el uso del almacenamiento local para conservar las preferencias;
   - los cambios de CSS o estilo para los modos de alto contraste y claro;
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado;
   - el código JavaScript o TypeScript que controla el cambio de modo.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar varios minutos.
10. **Opcional:** puedes cambiar a la rama de la PR en tu Codespace para probar manualmente las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por la rama que creó Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Después, inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Comprueba que funcionen y que la preferencia se conserve al volver a cargar la página.

El agente personalizado de accesibilidad ayudó a orientar a Copilot para implementar estas funciones según buenas prácticas de accesibilidad. Si detectas problemas o mejoras posibles, puedes mencionar a **@copilot** en un comentario y pedirle que los resuelva.

## Ejercicio opcional: sigue delegando

El agente en la nube es útil para asignarle tareas reales del backlog y ocuparte de otra cosa mientras trabaja. Para practicarlo, crea algunas incidencias en tu repositorio y asígnaselas a Copilot. Por ejemplo:

- Crear un formulario para que las personas interesadas apoyen un juego en la página de detalles.
- Implementar paginación en la página de la lista de juegos.
- Añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Revisaste los archivos de instrucciones personalizadas del repositorio** para conocer su efecto en el trabajo del agente.
- **Asignaste incidencias al agente de Copilot en la nube** y observaste cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Creaste y usaste un agente personalizado** de accesibilidad para añadir modos de alto contraste y claro.
- **Usaste la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad.
- **Revisaste las PR del agente e iteraste sobre ellas**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Repaso y siguientes pasos

Has completado el recorrido del agente en la nube. Si quieres seguir aprendiendo, los otros recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración de MCP desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y el puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
