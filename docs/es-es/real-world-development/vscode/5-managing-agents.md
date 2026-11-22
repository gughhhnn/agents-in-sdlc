---
title: "Ejercicio 5: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando GitHub Copilot está en modo de agente, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como este trabajo se realiza en tiempo real en tu equipo, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición de archivo a medida que ocurren. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y orientar la conversación con mensajes de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones de archivos propuestas.
- orientarás al agente durante la sesión para añadir un interruptor de modo claro junto al de alto contraste.

## Situación

El agente personalizado de accesibilidad que utilizaste en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha añadido una petición: también quiere un modo claro para quienes consideran que el tema predeterminado es demasiado oscuro. En lugar de iniciar otra conversación desde cero, orientarás la sesión existente para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso que sigue el agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición de archivo aparece en el chat con una diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar cada llamada a una herramienta. Verás lecturas y ediciones de archivos, y cualquier comando de terminal que pida ejecutar.
3. Cuando el agente proponga una edición, la diferencia aparecerá en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados por el agente durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta pide permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Orientar al agente para añadir el modo claro

Ya está implementado el modo de alto contraste. Ahora ampliarás la misma conversación para añadir un interruptor de modo claro. Como continuarás en la misma sesión de chat, el agente conservará todo el contexto del trabajo anterior: los componentes que modificó, el enfoque de estilo que siguió y el patrón de almacenamiento local que configuró.

1. En la entrada de Copilot Chat, envía este mensaje de seguimiento:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente continuará desde donde lo dejó, explorará los cambios anteriores y propondrá modificaciones para el modo claro.
3. Revisa cada nueva edición a medida que aparezca, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** en cada diferencia.
4. Si el agente toma una dirección que no quieres, por ejemplo, si intenta reorganizar la interfaz de los interruptores de una forma que no te convence, puedes detenerlo con el botón **Stop** junto a la entrada de chat y enviar un mensaje aclaratorio.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Usa mensajes de seguimiento para pedir ajustes («simplifica el CSS: usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer interruptor; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos interruptores»).

## Revisar el conjunto de cambios

Antes de confirmar el trabajo, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver actualizaciones de componentes Astro, estilos y pruebas relacionadas.
3. Abre varios archivos modificados y revisa las diferencias. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y persistencia en el almacenamiento local.
4. Cuando estés satisfecho, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición al llegar, aceptado o rechazado cambios y enviado mensajes de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección:

- has supervisado el trabajo de un agente en tiempo real.
- has aceptado y rechazado ediciones de archivos concretas.
- has orientado una sesión en curso mediante mensajes de seguimiento.

En la [lección siguiente][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de cambios y revisarás los cambios que haya producido el agente personalizado local durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
