---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, has pedido al agente en la nube de Copilot que se ocupe de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidad. Aunque está diseñado para trabajar de forma autónoma y asíncrona, sigue siendo importante supervisar estas tareas.

Dispones de varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida [la página de agentes][agents-page] de GitHub.com. Desde este panel de control puedes ver todas las tareas del agente que tienen solicitudes de cambios abiertas, consultar las operaciones realizadas y orientar las sesiones en curso para ayudar a dirigir el trabajo.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para solicitar funcionalidad adicional.

## Situación

Después de asignar al agente la creación de un modo de alto contraste, el equipo se dio cuenta de que también sería buen momento para añadir un modo claro. Como ya estaba trabajando en actualizar el estilo del sitio y añadir la función de alternancia, parecía lógico incluirlo. Orientarás el trabajo del agente para asegurarte de que añada tanto el modo claro como el alto contraste.

## Revisar las tareas del agente en la nube de Copilot

Veamos el estado actual de todas las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las listas de tareas del panel principal y del lateral. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - Actualizar la documentación del código base.
   - Generar API para modificar juegos.
   - Añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las tareas realizadas por Copilot, que pueden incluir:
   - Obtener el código del repositorio.
   - Crear el entorno de trabajo de Copilot.
   - Configurar servidores MCP.
   - Realizar varias operaciones para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos que aparecen varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitudes de cambios (PR) de la derecha. Desde ahí puedes ver la PR y los archivos modificados para seguir el trabajo.

## Orientar el agente en la nube

Ya has visto las tareas activas. Ahora vamos a pedir a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con la creación del modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado actual del trabajo.

   ![Sesión de accesibilidad en el panel de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este mensaje:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de pantalla de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para orientar la sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot lo confirma y lo incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Como antes, Copilot seguirá trabajando en la tarea actualizada. Incorporará la nueva petición al flujo de trabajo cuando termine el paso en el que se encontraba al recibir el mensaje.

El proceso también tardará varios minutos. Es un buen momento para hacer una pausa y reflexionar sobre todo lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro principal para supervisar y orientar las tareas del agente en la nube de GitHub Copilot. Desde este panel puedes hacer un seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir las tareas en curso para ajustar su alcance o proporcionar más indicaciones.

Has explorado estos conceptos:

- has explorado el panel de control y la página de agentes para supervisar las tareas del agente en la nube.
- has redirigido una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, podemos [iterar sobre las solicitudes de cambios creadas por Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
