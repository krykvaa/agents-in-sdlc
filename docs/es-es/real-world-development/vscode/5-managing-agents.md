---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo de agente de GitHub Copilot, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo y en tiempo real, Copilot Chat de VS Code muestra cada llamada a una herramienta y cada edición a medida que ocurren. Puedes revisar cada diff, aceptar o rechazar los cambios por separado y orientar la conversación con prompts de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás los cambios propuestos en los archivos;
- orientarás al agente durante la sesión para que añada un control de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha pedido también un control para el modo claro, pensado para las personas a quienes les resulta demasiado oscuro el tema predeterminado. En lugar de empezar una conversación nueva, ampliarás el trabajo desde la sesión actual.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a una herramienta. Aparecerán lecturas y ediciones de archivos y los comandos de terminal que quiera ejecutar.
3. Cuando el agente proponga una edición, aparecerá un diff en el chat:
   - selecciona **Keep** para aceptar el cambio;
   - selecciona **Undo** para deshacerlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados en la sesión.

> [!NOTE]
> Si una llamada a una herramienta necesita permiso —por ejemplo, para ejecutar un comando de terminal—, el agente se detendrá y mostrará la opción **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que termine de implementar el modo de alto contraste. Al final, mostrará un resumen de los cambios.

## Pide al agente que añada el modo claro

Ahora que está implementado el modo de alto contraste, amplía la misma conversación para añadir un control del modo claro. Como continuarás en la misma sesión, el agente conservará el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el patrón de almacenamiento local.

1. Envía este prompt de seguimiento desde el campo de Copilot Chat:

   ```plaintext
   ¡Buen trabajo! Ahora añade un control para el modo claro junto al control de alto contraste. El modo claro debe seguir el mismo patrón: conservarse mediante el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el modo claro y el modo de alto contraste de forma independiente.
   ```

2. El agente retomará el trabajo, revisará los cambios anteriores y propondrá los cambios necesarios para el modo claro.
3. Revisa cada nueva edición y selecciona **Keep** o **Undo** en cada diff.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, si intenta reorganizar los controles de una manera que no te gusta—, detenlo con el botón **Stop** junto al campo de chat y envía un prompt aclaratorio.

> [!TIP]
> Es más rápido orientar una sesión en curso que empezar de nuevo. Usa prompts de seguimiento para solicitar ajustes («simplifica el CSS y usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración actual») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa los archivos que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa los diffs. Confirma que se sigan las pautas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y orientar una sesión en curso. Has revisado las ediciones, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección:

- supervisaste el trabajo del agente en tiempo real;
- aceptaste y rechazaste ediciones individuales;
- orientaste la sesión con prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que ha producido el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
