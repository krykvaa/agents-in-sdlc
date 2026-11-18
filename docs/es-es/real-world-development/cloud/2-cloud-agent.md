---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog por falta de recursos. El agente de GitHub Copilot en la nube está diseñado para tareas como actualizar código y añadir funcionalidades de forma autónoma. Al terminar, crea una solicitud de incorporación de cambios (PR) en borrador para que una persona desarrolladora revise el trabajo. Así se pueden delegar tareas tediosas, acelerar el desarrollo y dedicar más tiempo a los problemas de mayor alcance.

Con el agente de Copilot en la nube explorarás:

- cómo personalizar el entorno de generación de código;
- cómo garantizar que las operaciones se realicen de forma segura;
- por qué es importante delimitar bien las incidencias;
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica pendiente. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: de hecho, no hay documentación. El equipo quiere empezar por añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la forma de descubrir juegos. En la página de detalles de cada juego quiere mostrar otros juegos de la misma categoría, para que las personas puedan seguir explorando. Por ahora no necesita una implementación pulida; solo algo que permita hacer pruebas de aceptación de la experiencia. Aunque esto bloquea el trabajo, hay otras incidencias con mayor prioridad.

Estas tareas pueden quedar relegadas fácilmente. Son buenas candidatas para asignarlas al agente de Copilot en la nube, que puede trabajar en segundo plano mientras la persona desarrolladora se ocupa de otras tareas y, después, revisar el trabajo y confirmar que cumple lo esperado.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a una persona desarrolladora. Puedes asignarle trabajo de distintas maneras, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna una tarea, Copilot crea una PR en borrador para registrar el progreso, prepara un entorno y empieza a trabajar. Puedes consultar la sesión mientras sigue activa o después de que termine. Cuando esté lista la solución propuesta, Copilot te mencionará en la PR para que la revises.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no elimina la necesidad de pensar. Incluso las tareas que parecen sencillas pueden ser complejas al analizarlas en detalle. Por eso, ten en cuenta las [buenas prácticas para delimitar tareas del agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como a una persona que programa contigo: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al incorporar IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya completaste el ejercicio práctico de instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción. Si no, este es un buen momento para revisar los archivos de instrucciones del repositorio antes de asignar trabajo al agente.

Abre estos archivos en la interfaz web de GitHub o en un Codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que limita estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Code lacks documentation`, observa si la PR incluye comentarios TSDoc, sigue las convenciones de TypeScript y añade encabezados de comentario. Estas indicaciones proceden de los archivos de instrucciones. Volveremos a revisarlas al revisar las PR en una lección posterior.

## Configura el entorno de desarrollo del agente

Crear código suele requerir un entorno específico y scripts de configuración para garantizar que todo esté preparado. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a una persona desarrolladora de software.

El agente usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Así puedes proporcionarle las herramientas y dependencias necesarias. Este taller ya incluye la configuración para que Copilot disponga de Node.js, las dependencias del proyecto, los navegadores de Playwright para las pruebas de extremo a extremo y la base de datos SQLite migrada y preparada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Concede solo los permisos mínimos necesarios para los pasos. Copilot recibe su propio token para sus operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo para instalar dependencias, necesitan el permiso `contents: read`. Si no lo clonan, Copilot lo hará automáticamente al terminar estos pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Preparación de la aplicación: Node.js (toda la aplicación ahora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y prepara la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero hay dos aspectos importantes:

- Tiene un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la PR.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Esa deuda técnica reduce la productividad y dificulta mantener el código o incorporar a nuevas personas al equipo. Copilot puede ayudar a crear documentación, por lo que esta incidencia es adecuada para asignársela al agente de Copilot en la nube. El agente trabajará en segundo plano y más adelante revisarás el resultado.

1. Abre tu repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

   ```plaintext
   Nuestra organización requiere que las funciones y los métodos incluyan comentarios de documentación TSDoc cuando sean útiles. Sin embargo, las actualizaciones recientes no han seguido este estándar. Tenemos que actualizar el código existente para añadir comentarios de documentación allí donde aclaren el comportamiento.
   ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva PR, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer. Copilot está preparando el entorno —ejecutando el flujo de trabajo anterior—, analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas que Copilot va a realizar.
14. Desplázate por la cronología de la PR. Deberías ver que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Desplázate por la sesión en directo y observa cómo Copilot explora el código, analiza su estado, decide un plan y crea código.

El proceso puede tardar varios minutos. Una ventaja importante del agente en la nube es que realiza tareas de forma asíncrona y te permite ocuparte de otros trabajos. Aprovecharás esa función asignándole otra tarea y, mientras tanto, escribirás código para añadir una funcionalidad.

## Añade una sección de juegos relacionados a la página de detalles

Una ventaja del agente de Copilot en la nube es que permite dividir el trabajo: puedes ocuparte de unas tareas mientras el agente trabaja en otras. Aunque añadir una sección de juegos relacionados quizá no lleve mucho tiempo, puedes aprovechar ese tiempo para otras tareas. Asígnale el trabajo al agente en la nube.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

   ```markdown
   Queremos ayudar a descubrir más juegos mostrando juegos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia y hacer pruebas de aceptación. Estos son los requisitos:

   - Añadir un helper de acceso a datos en `src/lib/` que devuelva otros juegos de la misma categoría que un juego, sin incluir ese juego
   - Mostrar una sección «Related games» en la página de detalles que use el helper
   - Gestionar el caso en que un juego no tenga juegos relacionados
   - Crear pruebas unitarias para el nuevo helper
   - Antes de crear la PR, comprobar que se superan todas las pruebas
   ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás un emoji 👀 en el primer comentario de la incidencia, que indica que Copilot está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente funciona de forma parecida a una persona desarrolladora: no tienes que supervisarlo constantemente, pero sí revisar el resultado cuando termine. Ahora puedes aprender a crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona. Puede ayudar a resolver deuda técnica, crear funcionalidades o migrar código entre frameworks.

Has aprendido:

- a personalizar el entorno de generación de código;
- a garantizar que las operaciones se realicen de forma segura;
- por qué es importante delimitar bien las incidencias;
- cómo asignar incidencias a Copilot.

Mientras el agente trabaja en segundo plano, puedes aprender a crear y usar agentes personalizados. El [agente en la nube también puede usar servidores MCP][cloud-agent-mcp] e instrucciones personalizadas, que ya has explorado en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
