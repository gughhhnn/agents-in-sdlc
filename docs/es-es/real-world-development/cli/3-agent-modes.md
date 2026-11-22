---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear los filtros a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto. Sin embargo, los cambios más amplios requieren un proceso más sólido. Por suerte, GitHub Copilot CLI está diseñado para trabajar con los flujos de trabajo existentes de una organización y ayudarnos a crear lo que necesitamos de la forma adecuada. Esta es la primera de varias lecciones en las que seguirás un proceso de desarrollo habitual basado en agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código sea válido y que la funcionalidad se comporte como se espera, y finalmente la combinarás correctamente con el proyecto.

> [!NOTE]
> Utilizarás la misma conversación y rama durante todo el flujo de trabajo de la funcionalidad. Normalmente se usarían ramas o PR distintas para los diferentes tipos de archivos, pero simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación nueva de Copilot a partir de una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás manualmente la funcionalidad en un navegador con el puerto reenviado.

A medida que avances, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Situación

El catálogo de Tailspin Toys crece y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia de la lista de trabajo describe la funcionalidad, pero antes de programar hay que acordar algunos detalles, como la combinación de categorías. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Información general

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los fundamentos. ¡De hecho, los hace aún más importantes! La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia registrada que describa el trabajo.
2. Crear un plan de lo que hay que desarrollar.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de cambios (PR).
7. Combinar el código después de que se revise y de que el proceso de integración continua se complete correctamente.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos siguen alguna variante del esquema anterior.

Al seguir este enfoque habitual, te aseguras de que el código generado por IA cumpla los requisitos y pase por el mismo proceso de evaluación que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** determina el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente. Este sugiere cambios y espera tu aprobación antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Partir de la incidencia

Antes de empezar a trabajar en los filtros, vuelve a tu Codespace y asegúrate de que el repositorio y el terminal estén listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha combinado la PR de las valoraciones con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para trabajar en los filtros:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al mensaje y volver a la pestaña **Session**.

Observa que el mensaje ahora empieza por `#7` (o un número parecido). El carácter `#` permite incorporar al contexto una incidencia o una solicitud de cambios (PR) de GitHub.

## Planificar la funcionalidad de filtros

La planificación permite definir el enfoque para implementar una funcionalidad o realizar tareas antes de asignárselas a Copilot. Siempre es recomendable dedicar algo de tiempo a planificar las tareas complejas. Cambia al modo Plan y pide a Copilot que cree un plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del mensaje muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, escribe el siguiente mensaje:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot se pondrá a trabajar en el plan. Primero explorará el proyecto y, después, determinará el mejor enfoque.

3. Puede que Copilot te pregunte cómo deberían funcionar los filtros. Responde según tus preferencias. ¡No hay respuestas incorrectas!
4. Cuando el plan esté listo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que revise cualquier parte del plan que no se ajuste a tus decisiones.

## Aprobar Autopilot

Ya has escrito y revisado el plan; ahora toca implementarlo. Deja que Copilot se encargue del trabajo con Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que está resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción de nombre parecido en la versión que tengas instalada.
2. Confirma que el indicador debajo del mensaje muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente, así que revísalo antes. Si Copilot informa de dependencias que faltan o de un conflicto de puertos, resuelve el problema de configuración antes de dar por terminadas las comprobaciones.

## Revisar y verificar la implementación

Cuando se genere el código, hay que revisarlo antes de combinarlo, como cualquier otro código. Revisaremos el código y ejecutaremos el sitio para comprobar que todo funcione.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador del modo ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación de los filtros y las pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva cualquier error.

## Explorar la nueva funcionalidad

Muy bien, el código parece correcto, pero ¿se ejecuta? Iniciemos la aplicación como antes y abramos el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editorial, así como las combinaciones que acordaste en el plan.
4. Confirma que el comportamiento de restablecimiento y de los resultados vacíos coincida con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una conversación nueva de Copilot a partir de una incidencia de GitHub.
- has definido los requisitos en el modo Plan.
- has implementado la funcionalidad con el modo Autopilot.
- has revisado el código.
- has validado manualmente la funcionalidad en un navegador con el puerto reenviado.

A continuación, profundizaremos en la generación de código para asegurarnos de que siga las prácticas documentadas mediante el uso de [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
