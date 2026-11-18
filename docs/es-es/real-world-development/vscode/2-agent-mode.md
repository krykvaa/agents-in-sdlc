---
title: "Ejercicio 2 - Añadir funcionalidades con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios más sencillos suelen requerir modificar varios archivos y realizar operaciones como ejecutar pruebas. El flujo habitual de desarrollo incluye localizar todos los archivos necesarios, hacer los cambios, ejecutar pruebas, depurar, averiguar qué archivo falta, volver a modificar el código y repetir el proceso.

El modo de agente de Copilot puede ayudarte con estas tareas.

El modo de agente funciona de forma autónoma en el IDE. Explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. En este ejercicio, usarás el modo de agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres sean parecidos, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en tu IDE y permite ciclos de respuesta rápidos e interacción directa. El agente en la nube trabaja de forma asíncrona como una persona del equipo e interactúa contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- usar el modo de agente de Copilot para explorar el proyecto, identificar los archivos pertinentes y coordinar cambios;
- implementar funcionalidades en la interfaz y la capa de datos;
- revisar los cambios y las pruebas generados por Copilot antes de combinar el código.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior añadiste un helper de editoriales. Ahora completarás el trabajo de la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con una temática de desarrollo. Permite mostrar juegos y consultar sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los helpers reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con scripts `<script>` de Astro pequeños y acotados, que usan las API estándar del DOM.

### Inicia el sitio

Hay una tarea `npm run dev` que inicia el único servidor de desarrollo de Astro.

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal dentro del Codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de Astro esté listo, verás un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección del servidor Astro `http://localhost:4321`.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost en el terminal se te redirige a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al Codespace, que ahora aloja el servidor web.

### Explora el sitio

Cuando el sitio esté en ejecución, explora sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios, vuelve al Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo pueden variar de un ejercicio a otro; cada paso te indicará qué seleccionar.

La primera versión del sitio funciona, pero queremos añadir capacidades. Empieza por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esas tareas.

1. En el menú de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos, ejecuta comandos del terminal y utiliza herramientas de forma autónoma.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Este taller no expresa preferencia por un modelo concreto. Durante su desarrollo se utilizó Claude Sonnet 4.5, por lo que aparece en estas instrucciones. El objetivo es que las sugerencias sean relativamente coherentes y la experiencia resulte útil. Sin embargo, como los modelos de lenguaje son probabilísticos, las sugerencias pueden diferir de las descritas; es normal y previsible.

> [!NOTE]
> Copilot puede utilizar un comando MCP diferente, pero debería poder completar la tarea.

3. Envía este prompt:

   ```plaintext
   Muéstrame el backlog de tareas de mi repositorio de GitHub. Ayúdame a priorizarlas según su utilidad para los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Copilot también habrá priorizado las tareas según la utilidad que considera que tienen para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que dará contexto a Copilot. Usarás el archivo de instrucciones de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio.

1. En el Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funcionalidades, entre ellas:
   - una descripción de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones específicos para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así puedes reutilizar contenido, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado hay que hacer al menos tres cambios en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar las pruebas y confirmar que se superan antes de combinar los cambios. El modo de agente de Copilot puede realizar estas tareas.

1. Continúa la conversación actual con Copilot o inicia otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú de agentes de la vista Chat.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad descrita en la incidencia correspondiente del backlog:

   ```plaintext
   Actualiza el sitio para incluir el filtrado por editorial y categoría según los requisitos de la incidencia de GitHub correspondiente del backlog. Ya hay un helper de editoriales del ejercicio anterior, así que conserva y ajusta ese trabajo cuando sea necesario mientras completas los cambios restantes en la capa de datos, la interfaz y las pruebas. Comprueba que todas las pruebas se superan antes de terminar. El servidor ya está en ejecución; no hace falta que lo inicies.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad. Debería encontrar los helpers de acceso a datos, la interfaz y las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot puede explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código, puede tardar varios minutos. Mientras trabaja, puede que aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás que aceptar o deshacer todos los cambios propuestos; no hace falta que selecciones los botones mientras sigue trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que solicita confirmación para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que Copilot se detenga varias veces o que algunas pruebas fallen durante el proceso. No pasa nada: alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión completa de Copilot Agent Mode en Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! Has usado el modo de agente de GitHub Copilot para añadir funcionalidades al sitio de Tailspin Toys. Has aprendido:

- cómo implementar funcionalidades en la interfaz y la capa de datos;
- cómo explorar el proyecto, identificar archivos pertinentes y coordinar cambios;
- cómo revisar los cambios y las pruebas generados antes de combinar el código.

Ahora [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario añadir paginación. Aplica lo que has aprendido para pedir a Copilot que implemente esta función. Ten en cuenta lo siguiente:

- sigue las buenas prácticas existentes, incluidos los archivos de instrucciones;
- decide cómo debe funcionar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo;
- proporciona a Copilot la orientación necesaria para obtener la implementación que quieres;
- puede que tengas que iterar con Copilot, pedir cambios y proporcionar contexto; es normal.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: qué hacen y cuándo se usan][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
