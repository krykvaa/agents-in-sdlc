---
title: "Lección 1 - Instalar la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio que has creado a partir de la plantilla, familiarízate con el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de CI funcionan sin configuración adicional. Está pensada para flujos de trabajo en los que diriges varios agentes en paralelo, cada uno en su propio espacio de trabajo aislado, en lugar de hacer todo el trabajo por tu cuenta, y automatizas tareas repetitivas. Con Node.js instalado y una copia del proyecto preparada, el siguiente paso es instalar la aplicación y conectar el repositorio.

En esta lección:

- instalarás la aplicación GitHub Copilot e iniciarás sesión;
- añadirás el proyecto a la aplicación desde su repositorio de GitHub;
- conocerás el espacio de trabajo y el backlog que creó la plantilla;
- probarás un chat rápido para conocer la propia aplicación.

## Escenario

Tu equipo está adoptando agentes de IA para abordar un backlog cada vez mayor. La aplicación Copilot ofrece un único lugar para dirigir ese trabajo: seleccionar incidencias, ejecutar agentes, revisar cambios y combinar solicitudes de incorporación de cambios. En esta lección instalarás y conectarás la aplicación y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Se necesita un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si utilizas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instala y configura la aplicación GitHub Copilot

Para usar la aplicación GitHub Copilot, primero tienes que instalarla. Hay versiones para Windows, macOS y Linux. Instala la aplicación, autentícate y añade el repositorio Tailspin Toys.

1. En un navegador, abre la [página de inicio de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de la página.
3. Abre la aplicación.
4. Selecciona **Sign in to GitHub** y sigue las indicaciones para iniciar sesión con una cuenta de GitHub.com o GHE.com. Si utilizas GitHub Enterprise Cloud con residencia de datos, elige **GitHub Enterprise Cloud** e introduce el nombre de host `*.ghe.com` cuando se te solicite.

   Si utilizas GitHub Enterprise Server, completa primero la incorporación con tu cuenta en la nube. Después, abre **Settings**, selecciona **Accounts**, selecciona **Add account** y elige **GitHub Enterprise Server**. Sigue las instrucciones para autorizar la aplicación en el navegador, como se describe en la [guía de inicio de sesión en Enterprise Server][ghes-sign-in].

5. Si el proceso de incorporación te permite seleccionar un repositorio, elige el de Tailspin Toys que creaste a partir de la plantilla. También puedes omitir este paso y añadir el proyecto más adelante.
6. Cuando se te solicite, elige un tema y termina el proceso de incorporación.
7. Busca el repositorio en **Sessions**, en la barra lateral. Si ya aparece, no hace falta añadirlo otra vez.
8. Si todavía no está conectado, selecciona **+** junto a **Sessions** para crear una sesión y, después, selecciona **Add GitHub repository**. Usa el propietario y el nombre de repositorio que anotaste en la lección de requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En este diseño, las carpetas de repositorio, como `tailspin-toys`, aparecen agrupadas en **Sessions**, con las sesiones individuales debajo de cada repositorio.

## Familiarízate con el espacio de trabajo

Con el proyecto conectado, dedica un momento a conocer el espacio de trabajo. La aplicación organiza sus funciones en varias áreas de la barra lateral:

- **New**: inicia una sesión de chat nueva con Copilot.
- **Pull requests**: tus solicitudes de incorporación de cambios, mediante la integración nativa con GitHub. Puedes explorarlas y filtrarlas, comprobar el estado de CI y revisarlas sin salir de la aplicación.
- **Issues**: tus incidencias. Puedes explorarlas y filtrarlas, e iniciar una sesión a partir de una incidencia.
- **Automations**: tareas de agente guardadas que se ejecutan según una programación o bajo demanda. Son útiles para listas de tareas, mantenimiento periódico u otras tareas tediosas. El repaso final las menciona como siguiente paso, no como otro ejercicio del taller.
- **Customize**: añade funciones a la aplicación Copilot mediante servidores MCP, plugins, habilidades y otros componentes. Lo utilizarás para configurar Playwright MCP.
- **Chats**: conversaciones ligeras para preguntas e ideas que no necesitan una rama ni un espacio de trabajo. Esta sección aparece después de iniciar el primer chat. Si no está visible, selecciona **+** junto a **Sessions** para iniciar un chat.
- **Sessions**: agrupa las carpetas de repositorio y sus sesiones de agente. Selecciona un repositorio, como `tailspin-toys`, para abrir su vista, o una sesión debajo para volver a esa conversación. Cada sesión se ejecuta en su propio espacio de trabajo aislado, así que puedes ejecutar varias a la vez sin que sus cambios entren en conflicto. Iniciarás tu primera sesión al añadir valoraciones con estrellas.

Durante el taller explorarás el espacio de trabajo.

> [!TIP]
> Si tienes dudas, pregunta a Copilot. Si no sabes cómo hacer algo o si es posible, puede ayudarte a orientarte.

### Busca el backlog creado para ti

Es probable que todos los proyectos tengan un backlog; Tailspin Toys no es una excepción. Vamos a explorar el backlog que se creó al usar la plantilla.

1. En **Sessions**, selecciona la carpeta del repositorio Tailspin Toys, no una sesión individual. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por su título, no por el número:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia para leer sus detalles. Cada incidencia también permite iniciar una sesión de agente. Después de un primer cambio rápido, empezarás por la incidencia de filtrado.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** se filtran automáticamente para mostrar solo elementos de los repositorios que has añadido a la aplicación Copilot. Para ver elementos de otros repositorios, añádelos a la aplicación.

## Prueba un chat rápido

Una buena forma de familiarizarse con la aplicación es utilizarla para conocer la *propia aplicación*. Para ello, un **chat rápido** es justo lo que necesitas. Permite hacer una pregunta o proponer ideas sin crear una rama ni un árbol de trabajo, así que sirve para una consulta rápida que no requiere una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat nuevo. Si **Chats** aún no aparece, selecciona **+** junto a **Sessions** para iniciar tu primer chat; después aparecerá **Chats**.
2. Pregunta a la aplicación cómo funcionan sus sesiones:

   ```plaintext
   ¿Cómo usa los árboles de trabajo la aplicación GitHub Copilot?
   ```

3. Lee la respuesta en la vista de conversación. Verás que cada sesión se ejecuta en su propio árbol de trabajo de Git aislado, lo que permite ejecutar varios agentes en paralelo sin que sus cambios entren en conflicto. Puedes continuar la conversación o iniciar otra en cualquier momento.

## Resumen y siguientes pasos

Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- instalar la aplicación e iniciar sesión en GitHub;
- añadir un proyecto desde su repositorio de GitHub;
- familiarizarte con el espacio de trabajo y encontrar el backlog en **Issues**;
- usar un chat rápido para formular una pregunta breve.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y la utilizarás para mostrar las valoraciones con estrellas en las tarjetas de juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Introducción a la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
