---
title: "Lección 9 - Comandos de barra de GitHub Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar tareas opcionalmente a la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye muchos comandos de barra. Estos comandos permiten acceder a funciones avanzadas, información interna y opciones de configuración adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Exploremos otros que pueden ser útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- usarás `/model` para explorar los modelos disponibles;
- aprenderás cómo `/share` puede exportar o compartir una sesión;
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

## Escenario

Has terminado el flujo principal de la CLI. Ahora explorarás otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y, si quieres, delegar trabajo al [agente de GitHub Copilot en la nube][about-cloud-agent].

## Explora el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, puede que alcances el tamaño máximo de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario. También puedes inspeccionar o compactar el contexto con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya es breve, es posible que no haya cambiado mucho.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando se llena. Usa `/compact` si quieres decidir cuándo hacerlo. Usa `/clear` o `/new` para iniciar una conversación limpia cuando cambies a una tarea no relacionada.

## Elige el modelo

Cada modelo tiene sus puntos fuertes y cada desarrollador sus preferencias. Copilot CLI permite mostrar los modelos disponibles y elegir uno.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos y la información de uso disponible.
3. Mantén el modelo actual, elige otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Trabajar en equipo y compartir lo aprendido ayuda a todo el mundo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir la sesión, elige el destino adecuado. Por ejemplo, `/share file` crea una exportación Markdown local.
3. Revisa el contenido exportado antes de enviárselo a alguien o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

No es obligatorio publicar un enlace o un gist. No publiques contenido del repositorio ni de la conversación que el equipo no tenga previsto compartir.

## Opcional: amplía el trabajo o delégalo

El flujo principal del taller ha terminado. Copilot CLI también ofrece comandos para tareas más amplias:

- `/fleet` puede dividir una tarea en subtareas independientes y ejecutarlas en paralelo mediante subagentes.
- `/worktree` puede crear un árbol de trabajo de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente de GitHub Copilot en la nube, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o trabajo remoto. Antes de probarlos, inicia una tarea nueva y bien acotada, y revisa los resultados con el flujo habitual. Para profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI te permiten configurarlo, compartir sesiones y ver qué ocurre internamente. En esta lección:

- usaste `/context` y `/compact` para explorar cómo gestiona Copilot el contexto;
- usaste `/model` para explorar los modelos disponibles;
- aprendiste cómo `/share` puede exportar o compartir una sesión;
- exploraste comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

Copilot CLI incluye muchos otros comandos de barra. Para cerrar el taller, [repasa lo que has aprendido][next-lesson] y explora los siguientes pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente de GitHub Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
