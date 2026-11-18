---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de Copilot en la nube →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información de fondo, conviene asegurarse de que dispone de ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan directrices para que Copilot entienda no solo *qué* quieres, sino también *cómo* quieres que lo haga.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos de rutas;
- añadirás un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no ejecutarás un prompt antes y otro después para comparar resultados. El agente de Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil demostrar su efecto en tiempo real. Verás cómo influyen tus instrucciones más adelante, al revisar las PR que genere el agente.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene un conjunto de directrices y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su contenido.

Con los archivos de instrucciones, te asegurarás de que Copilot disponga del contexto necesario para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot el contexto y las preferencias necesarios para que comprenda mejor tu estilo de programación y tus requisitos. Puedes orientar a Copilot para obtener sugerencias y fragmentos de código más útiles. Por ejemplo, puedes especificar convenciones de código, bibliotecas y los tipos de comentarios que prefieres. También puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

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
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses

## Explora los archivos de instrucciones personalizadas del proyecto

Este repositorio incluye un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción del proyecto y las secciones **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora su contenido. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros aspectos.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior define un patrón glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. Por ejemplo, coincidirá con cualquier archivo de prueba TypeScript que siga el patrón `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y mostrar a Copilot ejemplos que puede seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los siguientes pasos añadirás reglas para los comentarios TSDoc y los encabezados de comentario a nivel de archivo.

## Añade un nuevo estándar para el repositorio

El siguiente paso es el único cambio que harás en este ejercicio: añadir una regla general para que la documentación se incluya en el código mediante comentarios TSDoc y un encabezado de comentario a nivel de archivo. El agente en la nube la tendrá en cuenta cuando trabaje en incidencias que le asignes más adelante.

Antes de editar, crea una rama de trabajo. El agente en la nube leerá las instrucciones de la rama de destino de la incidencia, pero así podrás confirmar los cambios de forma ordenada:

1. En el terminal de tu Codespace, crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como ya se ha indicado, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Documenta los estándares del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de código del proyecto, pero aún no contiene una regla para la documentación dentro del código.
3. Añade estas líneas Markdown debajo de los estándares existentes para pedir a Copilot que añada encabezados de comentario a nivel de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, puedes crear instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para definir directrices generales, o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios de documentación que acabas de añadir.

## Confirma, envía y combina la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama de destino de la incidencia. En el siguiente ejercicio, asignarás una incidencia a Copilot y este creará una rama a partir de `main`. Por eso, los cambios de instrucciones deben llegar a `main`.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. Así, el agente en la nube leerá estas instrucciones cuando trabaje en las incidencias que le asignes en los ejercicios siguientes.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar los cambios directamente en `main`. El paso de la rama sirve para practicar un flujo de trabajo real.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has añadido un estándar para todo el repositorio a `.github/copilot-instructions.md`. Durante el resto del recorrido comprobarás cómo influye ese estándar en las PR que genera el agente en la nube.

A continuación, [asignarás tu primera incidencia al agente de GitHub Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
