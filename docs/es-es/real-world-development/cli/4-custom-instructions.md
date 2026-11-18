---
title: "Lección 4 - Guía a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene proporcionar a Copilot las instrucciones necesarias. Los [archivos de instrucciones][instruction-files] indican no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que ha utilizado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas;
- actualizarás el archivo de instrucciones para que se sigan los estándares de código;
- comprobarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene un conjunto de directrices y requisitos para sus prácticas de desarrollo:

- Los comentarios deben explicar la intención y las decisiones que no sean evidentes, no repetir el código.
- Las funciones exportadas en `db/` y `src/lib/` deben documentar su propósito, sus parámetros y los valores que devuelven mediante TSDoc/JSDoc; también deben documentar el argumento `db` inyectable cuando exista.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Deben conservarse las directrices de formato y lint existentes.

Con los archivos de instrucciones, te asegurarás de que Copilot disponga del contexto adecuado para realizar las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan a Copilot el contexto y las preferencias necesarias para que comprenda mejor tu estilo de desarrollo y tus requisitos. Te permiten orientar sus sugerencias y fragmentos de código. Puedes especificar convenciones de código, bibliotecas y los tipos de comentarios que prefieres. También puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot para **todas** las solicitudes del repositorio. Debe contener información del proyecto relevante para la mayoría de las solicitudes.
- `.github/instructions/*.instructions.md`, archivos que proporcionan directrices para lenguajes, tipos de archivo o tareas concretos.

> [!NOTE]
> La disponibilidad de otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad de las instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones personalizadas del proyecto

El proyecto de ejemplo ya incluye varios archivos de instrucciones. Explóralos antes de hacer cambios para ver qué orientación proporcionan.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la descripción del proyecto y sus directrices de código. Se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora su contenido. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros aspectos.
5. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior define un patrón glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en sus enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y mostrar a Copilot ejemplos que puede seguir al generar código.

> [!NOTE]
> La sección **Code formatting guidance** de `copilot-instructions.md` documenta los estándares de código del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentario a nivel de archivo.

## Actualiza los archivos de instrucciones según las directrices del equipo

Modifica el archivo principal `copilot-instructions.md` para exigir comentarios TSDoc en el nuevo código TypeScript.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade el siguiente elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones, cierra y reanuda la conversación de filtrado para que Copilot las cargue sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está activa la rama `game-filters-cli`.
3. Pide a Copilot que aplique las instrucciones actualizadas:

   ```plaintext
   Hemos actualizado las instrucciones y las directrices de código. ¿Puedes actualizar el código que has generado para que las siga?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Comprueba que los nuevos comentarios TSDoc sean precisos y describan el código correctamente.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has aplicado un nuevo estándar a la funcionalidad. En concreto:

- exploraste cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas;
- actualizaste el archivo de instrucciones para que se sigan los estándares de código;
- comprobaste el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para que las comprobaciones y pruebas se ejecuten de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Compatibilidad de las instrucciones personalizadas][custom-instructions-support]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
