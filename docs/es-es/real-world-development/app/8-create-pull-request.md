---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya combinaste manualmente la PR de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- inspeccionarás la PR completa de la funcionalidad y las pruebas de verificación;
- autorizarás Agent Merge solo después de la revisión y confirmarás que la PR se ha combinado.

## Escenario

Durante el trabajo de filtrado has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener la autorización para combinarla bajo el control de la persona desarrolladora.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para integrar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al activarlo, la sesión lee la PR y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y actualiza la base cuando hace falta. Después la combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, continúa después de reiniciar la aplicación y se desactiva cuando se combina la solicitud.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa tarea, pero su capacidad para editar código y combinar también requiere tu autorización explícita. Revisa las acciones que puede realizar y el trabajo antes de concederle permiso para combinar.

## Usa Agent Merge para gestionar la PR

Con todo el código creado y revisado, deja que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para elegir **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Se iniciará el proceso de Agent Merge. El agente:

- creará la solicitud de incorporación de cambios con un título y una descripción;
- si iniciaste la sesión desde una incidencia, la mencionará en el cuerpo de la descripción;
- actualizará la base o resolverá posibles conflictos de combinación con la rama de destino;
- supervisará el proceso de CI para confirmar que se superen todas las comprobaciones;
- supervisará los comentarios de revisión de otros desarrolladores o de Copilot y hará cambios para resolverlos;
- opcionalmente, combinará la PR automáticamente cuando todo se haya completado correctamente.

También permitiremos que Agent Merge combine la PR cuando se superen todas las comprobaciones.

5. Revisa el diff de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** está marcado.
8. Autoriza explícitamente al agente a combinar:

   ```plaintext
   Combina la PR cuando se superen todas las comprobaciones y aprobaciones obligatorias. No omitas las protecciones del repositorio.
   ```

9. Supervisa el progreso y resuelve los bloqueos que indique el agente. Confirma que el estado de la PR es **Merged** antes de continuar; tener activado el permiso de combinación o ver al agente en ejecución no confirma que la PR se haya combinado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varias partes del desarrollo: generar código, probarlo y validarlo, y ahora gestionar la solicitud de incorporación de cambios. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- inspeccionaste la PR completa de la funcionalidad y las pruebas de verificación;
- autorizaste Agent Merge solo después de revisar los cambios y confirmaste que la PR se había combinado.

A continuación, [usarás un canvas existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de inspeccionar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
