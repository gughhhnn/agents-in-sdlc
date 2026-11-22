---
title: "Ejercicio 2: Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muy pocas organizaciones, si es que hay alguna, no tengan problemas con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidad que llevan tiempo en la lista de trabajo porque nunca ha habido tiempo para implementarlas. El agente en la nube de GitHub Copilot está diseñado para realizar tareas como actualizar código y añadir funcionalidad de forma autónoma. Cuando termina, genera una solicitud de cambios en borrador para que la revise un desarrollador. Esto permite delegar tareas tediosas, acelerar el desarrollo y dejar que los desarrolladores se centren en cuestiones más importantes.

Con el agente en la nube de Copilot explorarás:

- cómo personalizar el entorno de generación de código.
- cómo garantizar que las operaciones se realicen de forma segura.
- por qué es importante definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

## Situaciones

Tailspin Toys tiene deuda técnica pendiente. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado poco adecuado: en realidad, no hay documentación. El primer paso es añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar cómo se descubren los juegos. Quiere que la página de detalles de cada juego muestre otros títulos relacionados, es decir, juegos de la misma categoría, para que las personas puedan seguir explorando. No necesita una implementación refinada todavía; solo algo que pueda utilizar para hacer pruebas de aceptación de la experiencia de usuario. Esta tarea está bloqueada, aunque hay otras incidencias de mayor prioridad.

Ambas son tareas que suelen perder prioridad y que resultan ideales para asignar al agente en la nube de Copilot. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas, y este puede volver más tarde para revisar el trabajo de Copilot y confirmar que todo está como se espera.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Como ocurre al trabajar con una persona, hay varias formas de hacerlo, entre ellas [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot creará una solicitud de cambios en borrador para hacer un seguimiento del trabajo, configurará un entorno y empezará la tarea. Puedes consultar la sesión de Copilot mientras está en curso o una vez finalizada. Cuando la solución propuesta esté lista para revisión, te mencionará en la solicitud de cambios.

## Importancia de instrucciones con un alcance bien definido

GitHub Copilot no hace magia: no puedes saltarte el trabajo de análisis. Incluso las operaciones que parecen sencillas pueden volverse complejas al examinarlas en detalle. Por eso, [define cuidadosamente el alcance de las tareas para el agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. La incorporación de IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores se han presentado las instrucciones personalizadas y cómo orientan a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción. Si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube de Copilot, revisa los archivos de instrucciones que ya incluye este repositorio para observar su efecto más adelante.

Abre los siguientes archivos en la interfaz web de GitHub o en un codespace si ya tienes uno abierto:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los requisitos de comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que limita estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la siguiente sección, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios en la solicitud de cambios resultante. Estas pautas proceden de los archivos de instrucciones que acabas de revisar. Volveremos a señalarlas al revisar las PR en un ejercicio posterior.

## Configurar el entorno de desarrollo del agente en la nube de Copilot

La creación de código suele requerir un entorno específico y la ejecución de algunos scripts de configuración para garantizar que todo esté en buen estado, independientemente de quién realice el trabajo. Lo mismo se aplica al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] para configurar el entorno de trabajo. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de empezar. Así podrá acceder a las herramientas y dependencias de desarrollo necesarias. El flujo ya está configurado para este laboratorio, para simplificarlo y facilitar el aprendizaje. Proporciona a Copilot Node.js, las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Asigna a estos pasos los permisos mínimos necesarios. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Para clonar el repositorio en los pasos de configuración, por ejemplo, para instalar dependencias, se necesita el permiso `contents: read`. Si no se clona el repositorio, Copilot lo hará automáticamente al terminar los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación: Node.js (la aplicación completa ahora usa Astro + Drizzle/libSQL).
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

      # Migra y precarga la base de datos SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

Se parece a cualquier otro archivo de flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de cambios.
- Fíjate en el activador `workflow_dispatch`, que permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Resulta útil para comprobar que funciona correctamente sin tener que esperar a que lo ejecute Copilot.

## Añadir documentación

Aunque todo el mundo reconoce la importancia de la documentación, la información de muchos proyectos está desactualizada o no existe. Este tipo de deuda técnica suele quedar sin resolver, reduce la productividad y dificulta mantener el código base o incorporar a nuevos miembros al equipo. Por suerte, Copilot destaca a la hora de crear documentación, así que esta es una incidencia perfecta para asignar al agente en la nube. El agente trabajará en segundo plano para generar la documentación necesaria. En un ejercicio futuro volverás para revisar el resultado.

1. Abre una pestaña nueva del navegador y ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

   ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de cambios (PR) recién generada, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece en la lista, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas de Copilot puede tardar varios minutos en aparecer en la PR. Copilot está configurando el entorno (ejecutando el flujo de trabajo mencionado), analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate hacia abajo por la cronología de la solicitud de cambios y busca una actualización que indique que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes desplazarte por la sesión en directo y ver cómo Copilot resuelve el problema: explora el código y su estado, se detiene a analizar y decidir el plan adecuado y crea código.

El proceso puede tardar varios minutos. Uno de los objetivos principales del agente en la nube de Copilot es que pueda realizar tareas de forma asíncrona y dejarte tiempo para ocuparte de otras cosas. Aprovecharemos esa función asignándole otra tarea y, después, nos centraremos en escribir código para añadir funciones a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Como ya hemos indicado, una de las grandes ventajas del agente en la nube de GitHub Copilot es que permite repartir el trabajo. Mientras tú te centras en unas tareas, el agente se ocupa de otras. Aunque añadir una sección de juegos relacionados para el equipo de diseño quizá no lleve mucho tiempo, puedes aprovecharlo para hacer otras tareas. ¡Vamos a asignársela al agente en la nube!

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás varios 👀 en el primer comentario de la incidencia. ¡Indican que Copilot ya se ha puesto manos a la obra!

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente en la nube de Copilot trabaja de forma parecida a un ingeniero de software. No hace falta supervisarlo activamente; puedes revisar el trabajo cuando haya terminado. Pasemos a crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona. Copilot puede abordar deuda técnica, crear funcionalidades o ayudar a migrar código de un framework a otro.

Has explorado estos conceptos:

- cómo personalizar el entorno de generación de código.
- cómo garantizar que las operaciones se realicen de forma segura.
- por qué es importante definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

Mientras el agente en la nube trabaja en segundo plano, puedes centrarte en crear y utilizar agentes personalizados. [El agente en la nube de Copilot también puede usar servidores MCP][cloud-agent-mcp] y puede acceder a las instrucciones personalizadas que exploramos en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignación de incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]

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
