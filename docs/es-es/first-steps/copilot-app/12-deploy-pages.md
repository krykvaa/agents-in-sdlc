---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y comprobar el Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica el Space Quiz para que cualquiera pueda visitarlo. Copilot puede preparar el flujo de trabajo y guiarte durante la publicación, pero tú eres responsable de revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure la publicación en GitHub Pages;
- revisarás el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla;
- seguirás la publicación y comprobarás el sitio publicado.

## Escenario

El Space Quiz ya está en un repositorio de GitHub, pero la gente todavía necesita una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él mediante una URL pública sin añadir un servidor ni dependencias de ejecución.

## Pide a Copilot que prepare la publicación

> [!WARNING]
> Crear una solicitud de incorporación de cambios puede confirmar y enviar código. Revisa la configuración de aprobación y el repositorio de destino antes de enviar este prompt. Si está activada la opción **Approve all**, no esperes otro mensaje de confirmación.

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Prepara la publicación de este repositorio en GitHub Pages. Crea un flujo de trabajo de GitHub Actions que compile el sitio si es necesario y lo publique en GitHub Pages cuando los cambios lleguen a la rama predeterminada. Añade un README que describa la aplicación y cómo ejecutarla en local, si todavía no existe. Crea una solicitud de incorporación de cambios con estos cambios e indica su enlace y cualquier configuración o permiso necesario de Pages. No combines la solicitud, no actives la combinación automática, no cambies la configuración de Pages ni publiques el sitio. Detente y espera a que lo revise.
```

## Revisa el flujo de trabajo y la solicitud de incorporación de cambios

1. Confirma que el archivo README solo se añade si el repositorio aún no tiene uno.
2. Revisa los cambios de la solicitud de incorporación de cambios, incluidos los pasos de compilación y publicación del flujo de trabajo, los desencadenadores y los permisos.
3. Confirma que el flujo de trabajo publica desde la rama predeterminada, no desde una solicitud de incorporación de cambios que aún no se haya combinado.
4. Solicita las correcciones necesarias y revisa el diff actualizado. Espera a que se superen las comprobaciones obligatorias de la solicitud de incorporación de cambios antes de autorizar su combinación.

## Autoriza la combinación y la publicación

> [!WARNING]
> El prompt siguiente autoriza al agente a combinar la solicitud de incorporación de cambios revisada, cambiar la configuración de Pages del repositorio y publicar el sitio. Envíalo solo después de terminar la revisión y confirmar el repositorio y la solicitud de incorporación de cambios. Según la configuración de aprobación, estas acciones podrían realizarse sin otra confirmación.

Sustituye `<PR_NUMBER>` por el número de la solicitud de incorporación de cambios que revisaste y, después, envía este prompt por separado:

```plaintext
He revisado la solicitud de incorporación de cambios <PR_NUMBER> de este repositorio y autorizo su combinación y la publicación. Habilita GitHub Pages mediante GitHub Actions, combina esa solicitud solo cuando se superen las comprobaciones obligatorias y supervisa la publicación hasta que termine correctamente. Indica la URL final del sitio publicado. Si no tienes permiso para habilitar Pages o combinar la solicitud, dime exactamente qué debo aprobar o hacer; no informes de que la tarea se ha completado hasta que el sitio esté publicado.
```

![Diagrama de tres etapas: una solicitud de incorporación de cambios de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions, con los pasos Build website, Upload artifact y Deploy to Pages; y el Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Comprueba la publicación

1. Sigue la ejecución de publicación de GitHub Actions hasta que termine correctamente.
2. Abre la URL indicada y confirma que se carga el Space Quiz.

Si el agente no puede habilitar Pages o combinar la solicitud de incorporación de cambios, sigue las instrucciones específicas que te proporcione. No des la tarea por terminada hasta que la publicación haya finalizado correctamente y el sitio esté disponible.

## Resumen y siguientes pasos

Ahora el Space Quiz está disponible como sitio publicado y has comprobado la publicación en lugar de confiar solo en el informe del agente. Continúa con la [Lección 13: Repaso y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
