---
title: "Lección 10 - Repaso y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI en un flujo de trabajo continuo de Tailspin Toys. En este taller:

- preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y localizaste la incidencia de filtrado preparada;
- añadiste valoraciones con estrellas, revisaste el resultado en un navegador reenviado y combinaste manualmente tu primera solicitud de incorporación de cambios (PR);
- empezaste por la incidencia de filtrado, definiste el enfoque con el modo Plan, lo implementaste con Autopilot y lo revisaste en el modo Interactive;
- orientaste al agente con instrucciones personalizadas, personalizaste la habilidad existente `quality-checks` y la usaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- añadiste el servidor Model Context Protocol (MCP) de Playwright y lo usaste para explorar el filtrado en un navegador real;
- creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas del navegador;
- revisaste el cambio completo del filtrado y autorizaste Agent Merge para su PR;
- exploraste comandos de barra para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Lo que has publicado

El taller incluye dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizada:

1. **Valoraciones con estrellas:** muestra `starRating` y un estado explícito para los juegos sin valorar en las tarjetas.
2. **Filtrado y flujo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, usaste la misma conversación y rama. Combinamos ese trabajo en una sola PR para simplificar el taller.

## Diferentes tipos de verificación

Comprobaste el código de varias maneras: con pruebas automáticas, inspeccionándolo en el navegador y usando Copilot para explorar el navegador mediante MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, el lint y las comprobaciones de tipos, y los presentó en el nuevo formato. El agente de QA reunió esos resultados con una revisión de los requisitos y de la cobertura antes de abrir la PR.

Las pruebas que se añadan deben cubrir carencias reales; si QA concluye que no hace falta añadir ninguna, también puede ser correcto. Revisa el código y las pruebas y, después de cada cambio, actualiza las pruebas que hayan quedado obsoletas.

## Buenas prácticas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller actualizaste instrucciones, personalizaste una habilidad, creaste un perfil de QA y configuraste un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y adáptalas a las necesidades del equipo. Las instrucciones definen estándares, las habilidades describen tareas repetibles, los agentes personalizados establecen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas; no te limites al resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para pensar el enfoque antes de empezar, **Interactive** para colaborar en cambios concretos y **Autopilot** para tareas bien acotadas. Elige un modelo más rápido para ediciones rutinarias y uno más capaz para trabajos complejos.

El contexto sigue siendo tan importante como la infraestructura. Explicar con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más cosas por explorar

Ya has cubierto el flujo principal. También puedes explorar otras funciones de la CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar subtareas independientes en paralelo;
- `/worktree` para aislar otra tarea;
- `/delegate` para enviar una tarea al agente de GitHub Copilot en la nube.

## Siguientes pasos

La mejor forma de mejorar con una herramienta es seguir usándola. Úsala para código de producción, proyectos personales o esa pequeña aplicación que llevas años queriendo crear. Comparte lo que aprendas con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
