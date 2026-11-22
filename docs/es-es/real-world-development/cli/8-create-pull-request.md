---
title: "Lección 8 - Crear y combinar la PR de funcionalidad"
description: "Revisa los filtros, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación de los filtros, las actualizaciones de las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Ahora toca revisarlos en conjunto y abrir una solicitud de cambios. Combinaste personalmente la PR de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de trabajo de filtros y calidad en una conversación y rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionarás la funcionalidad completa y las pruebas de verificación.
- crearás la PR de los filtros.
- activarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha combinado.

## Situación

A lo largo del flujo de trabajo de los filtros, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener el control de la autorización para combinarla en manos del desarrollador.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de cambios. Al activarlo, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y vuelve a aplicar los cambios sobre la rama base cuando hace falta. Después, activa la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado personalmente **Merge pull request**. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y asegúrate de que está listo antes de activarlo.

## Usar Agent Merge para gestionar la PR

Ya has creado todo el código. Vamos a revisarlo, crear la PR y permitir que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una nueva PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo salga bien, la combinará.
7. Comprueba que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no elude las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varios aspectos del proceso de desarrollo: la generación de código, las pruebas y la validación del código, y ahora el proceso de la solicitud de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- has inspeccionado la funcionalidad completa y las pruebas de verificación.
- has creado la PR de los filtros.
- has activado Agent Merge solo después de revisar los cambios y has confirmado que la PR se ha combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Recursos

- [Gestión de solicitudes de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
