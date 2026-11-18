---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear el filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. En lugar de probar la aplicación manualmente, conectarás el **servidor Playwright MCP** y dejarás que Copilot controle un navegador real. Después, publicarás la rama y abrirás una PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas de Copilot;
- añadirás el servidor Playwright MCP al área de trabajo;
- pedirás a Copilot que lo use para probar manualmente el filtrado en un navegador;
- publicarás la rama y abrirás una solicitud de incorporación de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es más potente cuando puede acceder a herramientas fuera del editor. Copilot lo consigue mediante Model Context Protocol (MCP), un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las herramientas de servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

El agente accede a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente y los servicios externos. El servidor gestiona la comunicación con herramientas externas, como API existentes o herramientas locales como paquetes de NPM. Cada servidor ofrece un conjunto diferente de herramientas y recursos.

Algunos servidores MCP conocidos son:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a API para gestionar repositorios de GitHub, crear repositorios, actualizarlos y administrar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: permite automatizar el navegador con Playwright, por ejemplo, navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que proporcionan acceso a diferentes herramientas y recursos. GitHub aloja un [registro MCP](https://github.com/mcp) que facilita descubrir servidores y contribuir al ecosistema.

> [!CAUTION]
> Por motivos de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad. Usa solo servidores en los que confíes y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en esta área de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Deberías encontrar un servidor `github` ya configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

La entrada `github` forma parte de la plantilla del proyecto. Por eso Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hace falta instalarlo en local. VS Code se conecta mediante HTTP y te autenticas con GitHub la primera vez que Copilot usa una de sus herramientas.

## Añade el servidor Playwright MCP

Ahora añadirás un segundo servidor. El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es **local**: VS Code lo inicia en tu equipo con `npx @playwright/mcp@latest`. La opción `--headless` hace que Playwright ejecute el navegador sin mostrar una ventana, algo necesario en un Codespace, que no tiene un escritorio donde mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, así que normalmente el navegador necesario ya está instalado. Si Copilot indica que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Inicia el servidor y confirma que confías en él

VS Code inicia los servidores MCP **cuando se necesitan**. La primera vez que Copilot requiere las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, aparecerá una acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para permitir que se inicie.

Iniciarlo manualmente es opcional. Si lo omites, VS Code iniciará el servidor y te pedirá que confirmes que confías en él la primera vez que Copilot necesite sus herramientas.

## Prueba la funcionalidad de filtrado

El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador real que puede controlar. En lugar de probar la aplicación tú mismo, el agente puede abrir una página, navegar por ella, aplicar filtros y leer los resultados. Después, te resumirá lo que ha observado. Así puedes comprobar rápidamente el comportamiento de la funcionalidad sin salir de la conversación.

El servidor Playwright MCP utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. El agente trabaja con elementos estructurados y etiquetados, como botones, enlaces y elementos de lista, igual que las tecnologías de asistencia. Por eso una comprobación funcional también permite hacer una revisión básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe el filtrado:

```text
Usa el servidor Playwright MCP para abrir un navegador con la aplicación en ejecución en http://localhost:4321 y comprobar la nueva funcionalidad de filtrado de juegos:

1. Ve a la página de juegos e indica cuántos juegos aparecen.
2. Aplica un filtro de categoría y confirma que la lista muestra solo los juegos de esa categoría.
3. Borra el filtro y, después, aplica un filtro de editorial. Confirma que la lista muestra solo los juegos de esa editorial.
4. Combina un filtro de categoría y otro de editorial y confirma que los resultados cumplen ambos.

Describe lo que observes en cada paso e indica cualquier comportamiento inesperado.
```

Copilot abrirá un navegador mediante Playwright MCP, realizará los pasos y te informará de los resultados. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona, haz preguntas de seguimiento o pide a Copilot que corrija el código antes de abrir una PR.

> [!NOTE]
> Para esta prueba, la aplicación debe estar en ejecución en `http://localhost:4321`. Si has detenido el servidor de desarrollo, inícialo de nuevo antes de enviar el prompt. La primera vez que Copilot use Playwright MCP puede tener que descargar un navegador. Si indica que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Has confirmado que la funcionalidad funciona. Ahora abre una solicitud de incorporación de cambios (PR) para que el equipo pueda revisarla. Primero publica la rama `filtering-vscode`.

1. Abre el panel **Source Control** del Codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Puedes crear una PR desde github.com o con la interfaz de línea de comandos de GitHub (CLI). Como ya trabajas con GitHub Copilot, pídele que la cree. Puede localizar la incidencia pertinente y asociarla a la PR.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva.
2. Confirma que **Agent** está seleccionado en el menú de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con el filtrado por categoría y editorial. Crea una solicitud de incorporación de cambios para la rama actual y relaciónala con la incidencia correcta.
   ```

4. Cuando Copilot lo solicite, selecciona **Continue** para permitirle recopilar información y realizar las operaciones necesarias. La primera vez que use una herramienta de GitHub, puede que tengas que autenticarte; sigue las instrucciones para hacerlo.
5. Observa cómo Copilot busca entre las incidencias, encuentra la adecuada y crea la PR.
6. Selecciona el enlace que genera Copilot para revisar la PR, pero **no la combines todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- aprendiste qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot;
- añadiste el servidor Playwright MCP al área de trabajo;
- lo usaste para probar manualmente la funcionalidad de filtrado en un navegador antes de publicarla;
- publicaste la rama y abriste una solicitud de incorporación de cambios para el trabajo de filtrado.

Has usado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una manera de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas concretas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
