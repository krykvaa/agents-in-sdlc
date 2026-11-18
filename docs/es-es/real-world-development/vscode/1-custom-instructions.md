---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información de fondo, conviene asegurarse de que dispone de ese contexto. Hay varias formas de compartir información específica con Copilot. Una de las más importantes son los [archivos de instrucciones][instruction-files], que proporcionan directrices sobre la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos de rutas;
- generarás el primer bloque de datos para el filtrado (un helper de editoriales) con las instrucciones *actuales*;
- añadirás un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`;
- volverás a ejecutar el mismo prompt y observarás cómo el código generado sigue el nuevo estándar;
- confirmarás los cambios de instrucciones y el bloque de filtrado en `main` para que Copilot pueda utilizar las instrucciones actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares que establezcas. Copilot no es determinista. El objetivo es observar la *tendencia* de cambio en el comportamiento después de actualizar las instrucciones, no obtener exactamente el mismo código carácter por carácter.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene un conjunto de directrices y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su contenido.

Con los archivos de instrucciones, te asegurarás de que Copilot disponga del contexto adecuado para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot el contexto y las preferencias necesarios para que comprenda mejor tu estilo de programación y tus requisitos. Te permiten orientar sus sugerencias y fragmentos de código. Puedes especificar convenciones de código, bibliotecas y los tipos de comentarios que prefieres. También puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot para **todas** las solicitudes del repositorio. Debe contener información del proyecto relevante para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción general de lo que se desarrolla, buenas prácticas y otras directrices generales.
- `.github/instructions/*.instructions.md`, archivos que puedes crear para tareas o tipos de archivo concretos. Permiten definir directrices para lenguajes como TypeScript o Astro, o para tareas como crear un componente de interfaz o un conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones se utilizan para generar código en Copilot Chat, pero no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Además, Copilot [admite archivos de instrucciones que siguen otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

Este taller no aborda en detalle cómo crear archivos de instrucciones, pero los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén `copilot-instructions.md` centrado en directrices del proyecto, como qué se está desarrollando, cómo está organizado el proyecto y cuáles son los estándares generales de programación.
- Usa `*.instructions.md` para ofrecer directrices específicas de tipos de archivo (pruebas unitarias, componentes Astro, capa de datos) o de tareas.
- Usa lenguaje natural, escribe directrices claras y proporciona ejemplos de código correcto e incorrecto.

No hay una única manera de crear archivos de instrucciones, igual que no hay una única manera de usar la IA. La experimentación te ayudará a encontrar el enfoque adecuado para tu proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería contar con una colección sólida de archivos de instrucciones. Al explorar los del proyecto, verás que hay archivos para numerosos tipos de tareas, como las [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El taller del entorno que estés usando te lo indicará cuando sea pertinente.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization

## Explora los archivos de instrucciones personalizadas del proyecto

Este repositorio incluye un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción del proyecto y las secciones **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora su contenido. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros aspectos.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior define un patrón glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. Por ejemplo, coincidirá con cualquier archivo de prueba TypeScript que siga el patrón `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y mostrar a Copilot ejemplos que puede seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los siguientes pasos añadirás reglas para los comentarios TSDoc y los encabezados de comentario a nivel de archivo.

## Crea una rama para los cambios

Sigue buenas prácticas y crea una rama para los cambios.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo pueden variar de un ejercicio a otro; cada paso te indicará qué seleccionar.

Para observar el efecto de las instrucciones personalizadas, envía primero un prompt con las instrucciones actuales. Más adelante, las actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot reciba solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el helper.
3. En el menú de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar cambios a los archivos.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva una lista de todas las editoriales, con el nombre y el id de cada una. Aplica los cambios a los archivos.
   ```

5. Copilot explorará el proyecto y aplicará los cambios, que pueden afectar al helper y a sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve un array tipado de editoriales. Esto se debe a las convenciones de la capa de datos en `.github/instructions/drizzle.instructions.md`, que se aplican a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** comentarios TSDoc y un encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Copilot es probabilístico, así que puede que añada comentarios de documentación aunque no se lo pidas. No pasa nada; lo importante es mejorar la *coherencia* después de actualizar las instrucciones.

## Añade un nuevo estándar para el repositorio

El archivo `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Añade los estándares de programación del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de programación del proyecto, pero aún no incluye reglas para documentar el código.
3. Añade las siguientes líneas Markdown debajo de los estándares existentes para pedir a Copilot que añada encabezados de comentario a nivel de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, puedes crear instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para definir directrices generales, o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios de documentación que acabas de añadir.

## Vuelve a ejecutar el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar la conversación.
2. Selecciona de nuevo `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía el **mismo prompt** que antes:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva una lista de todas las editoriales, con el nombre y el id de cada una. Aplica los cambios a los archivos.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Helpers de acceso a datos de editoriales para la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su id y su nombre.
    *
    * @param db - El cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en un array de objetos de editoriales.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar del proyecto y lo aplique a código real que utilizarás en el ejercicio siguiente.

## Confirma las instrucciones y envía la rama

Los archivos de instrucciones, como cualquier otro archivo del repositorio, se gestionan mediante el control de versiones. Confirma los cambios y envía la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Ejecuta este comando para comprobar las instrucciones y los cambios del helper:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y todos los archivos modificados por Copilot para crear el helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados, como pruebas, añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Ya has enviado la rama. Ahora abre una solicitud de incorporación de cambios (PR) y relaciónala con la incidencia sobre los estándares de documentación del backlog. Copilot puede buscar la incidencia, crear una PR que la cierre y, después, combinarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub en tu nombre. La plantilla del proyecto ya conecta las herramientas de GitHub al área de trabajo, así que no hace falta configurarlas. La primera vez que Copilot use una herramienta, VS Code podría pedirte que inicies sesión en GitHub. Sigue las instrucciones para autorizarlo. Más adelante aprenderás cómo funciona esta conexión con Model Context Protocol.

1. Abre Copilot Chat en tu Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y cree una PR desde la rama actual. Indícale que describa tanto las instrucciones como el helper inicial de editoriales:

   ```plaintext
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una solicitud de incorporación de cambios desde la rama actual, indica que la PR cierra esa incidencia y explica que también hemos añadido la base del helper de editoriales para el próximo trabajo de filtrado.
   ```

4. Copilot empezará a buscar la incidencia y a crear la PR.
5. Si Copilot te pide **Allow** para realizar acciones de GitHub, revisa el comando y selecciona **Allow** cuando corresponda.
6. Cuando se cree la PR, pídele que la combine en `main` y vuelva a esa rama local:

   ```plaintext
   Combina la PR en main. Después, vuelve localmente a main y descarga el código más reciente para actualizar el repositorio.
   ```

7. Si Copilot te pide **Allow** para realizar acciones de GitHub o ejecutar comandos de shell, revisa el comando y selecciona **Allow** cuando corresponda.

¡Has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y, después, has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*;
- añadir un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`;
- volver a ejecutar el mismo prompt y observar cómo el código propuesto adopta el nuevo estándar;
- confirmar las instrucciones y el helper inicial en `main` para que el siguiente ejercicio pueda aprovecharlos.

A continuación, aplicarás estas instrucciones en el [modo agente][next-lesson] mientras Copilot añade una funcionalidad al código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Instrucciones personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
