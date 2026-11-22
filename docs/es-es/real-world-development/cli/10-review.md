---
title: "Lección 10 - Recapitulación y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI a lo largo de un flujo de trabajo continuo en Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia precargada sobre filtros.
- añadido valoraciones con estrellas, revisado el resultado en un navegador con el puerto reenviado y combinado manualmente tu primera solicitud de cambios (PR).
- partido de la incidencia sobre filtros, definido el enfoque en el modo Plan, creado la funcionalidad en el modo Autopilot y revisado el resultado en el modo Interactive.
- guiado al agente con instrucciones personalizadas, personalizado la habilidad existente `quality-checks` y utilizado esta para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadido el servidor Model Context Protocol (MCP) de Playwright y utilizado este para explorar los filtros en un navegador real.
- creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador.
- revisado el cambio completo de los filtros y autorizado Agent Merge para la PR de filtros.
- explorado comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Lo que has entregado

El taller tiene dos hitos de PR, cada uno en su propia rama basada en `main` actualizado:

1. **Valoraciones con estrellas:** muestra `starRating` existente y un estado explícito de «sin valoración» en las tarjetas de juegos.
2. **Filtros y flujo de calidad:** implementa los filtros, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas asociadas.

Desde la planificación de los filtros hasta la apertura de su PR, has utilizado la misma conversación y rama. Hemos agrupado ese trabajo en una sola PR para simplificar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias formas: pruebas automatizadas, inspección propia en el navegador y exploración del navegador por parte de Copilot mediante MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y presentó los resultados en el nuevo formato. Antes de abrir la PR, QA reunió esos resultados con una revisión de los requisitos y la cobertura de pruebas.

Las pruebas añadidas deben cubrir carencias reales; una ejecución de QA que no requiera pruebas nuevas también puede ser correcta. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las pruebas afectadas después de hacer cambios.

## Buenas prácticas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller, has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas a medida que cambien las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen funciones especializadas y los servidores MCP conectan herramientas externas. Revisa los cambios reales y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para la tarea. Usa **Plan** para definir un enfoque antes de construir, **Interactive** para seguir participando en cambios concretos y **Autopilot** para tareas bien acotadas. Elige un modelo más rápido para ediciones rutinarias y uno más capaz para trabajos complejos.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* cambia sustancialmente el resultado.

## Más opciones para explorar

Ya has cubierto el flujo de trabajo principal. Estas otras funciones de CLI también pueden resultarte útiles:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para analizar un problema y obtener otra perspectiva.
- `/fleet` para coordinar subtareas independientes en paralelo.
- `/worktree` para aislar una tarea aparte.
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Úsala con código de producción, en proyectos personales o para esa pequeña aplicación que llevas años imaginando y no habías llegado a crear. Comparte lo que aprendas con tu equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalización de Copilot CLI][customize-cli]
- [Gestión de solicitudes de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
