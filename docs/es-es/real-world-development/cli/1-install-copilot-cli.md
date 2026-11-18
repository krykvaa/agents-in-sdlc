---
title: "Lección 1 - Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, familiarízate con la herramienta y busca la incidencia de filtrado preparada."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en el terminal y permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas desde la línea de comandos. Puedes delegar tareas, solicitar cambios y mantener la concentración. El primer paso es instalar la herramienta, algo que puedes hacer con herramientas que ya conoces.

En esta lección:

- instalarás GitHub Copilot CLI con npm;
- te autenticarás con tu cuenta de GitHub;
- confiarás en el repositorio del taller y probarás una conversación rápida;
- buscarás la incidencia de filtrado mediante el servidor GitHub MCP integrado.

## Escenario

Tu equipo está empezando a usar agentes de IA para abordar un backlog cada vez mayor. Copilot CLI lleva esa capacidad al terminal, donde muchos desarrolladores ya trabajan. En esta lección instalarás la herramienta, te autenticarás y la dejarás lista para utilizarla durante el resto del taller.

## Instala Copilot CLI

Puedes instalar Copilot CLI con [npm][install-cli], WinGet o Homebrew. Como GitHub Codespaces incluye Node.js, usarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js está instalado y cumple el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o posterior.

3. Instala Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Debería mostrarse el número de versión.

## Autentícate con GitHub

La primera vez que inicies Copilot CLI, te pedirá que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se solicita, sigue las instrucciones para autenticarte con el código de dispositivo y autorizar Copilot CLI.
3. Copilot CLI mostrará este prompt:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Comprueba que la ruta corresponde al repositorio Tailspin Toys y responde que sí seleccionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En Codespaces, puede que ya estés autenticado mediante tu sesión de GitHub. Si Copilot CLI se inicia sin pedir autenticación, ya está listo para usarse.

## Familiarízate con la herramienta

Los comandos que escribes en el prompt habitual del terminal se ejecutan directamente en Codespaces. Cuando se inicia Copilot CLI, el lenguaje natural se envía al agente y los comandos de barra controlan la conversación.

1. Escribe `/model`, usa las teclas de flecha para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos disponibles en tu versión y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla para comprobar que todo funciona:

   ```plaintext
   ¿Cuáles son los archivos principales de este proyecto?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Escribe `/mcp list` y confirma que el servidor GitHub MCP integrado está disponible.
6. Pide a Copilot que busque la incidencia de filtrado:

   ```plaintext
   Usa GitHub MCP para buscar en este repositorio la incidencia titulada "Allow users to filter games by category and publisher". Dame su URL y un breve resumen. No cambies nada.
   ```

7. Abre la URL y lee la incidencia. La utilizarás después de completar un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI utiliza la rama que esté activa en el terminal; no crea un árbol de trabajo automáticamente. Crearás una rama de funcionalidad antes de cada cambio.

## Usa el atajo del taller

Normalmente, Copilot CLI pide permiso antes de utilizar herramientas que no estén incluidas en los permisos existentes. En este taller volverás a iniciarlo con `--yolo`, un atajo aprobado por el usuario que elimina esos avisos dentro del Codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` habilita permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un Codespace o una máquina virtual, y nunca lo configures como alias predeterminado para el trabajo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para obtener más información.

Durante el taller, `--enable-all-github-mcp-tools` habilita las herramientas GitHub MCP de lectura y escritura que se utilizarán en lecciones posteriores para trabajar con incidencias y solicitudes de incorporación de cambios. El Codespace limita el acceso a tu equipo local, pero las operaciones autenticadas de GitHub siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarlo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta rápida sobre el proyecto para confirmar que funciona y sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, después de cambiar instrucciones o añadir un agente, utilizarás `copilot --resume` para volver a la misma conversación y rama de la funcionalidad.

## Resumen y siguientes pasos

En esta lección:

- instalaste GitHub Copilot CLI con npm;
- te autenticaste con tu cuenta de GitHub;
- confiaste en el repositorio del taller y probaste una conversación rápida;
- encontraste la incidencia de filtrado mediante el servidor GitHub MCP integrado.

A continuación, [empezarás tu primer cambio acotado][next-lesson] y usarás Copilot CLI para mostrar una valoración con estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
