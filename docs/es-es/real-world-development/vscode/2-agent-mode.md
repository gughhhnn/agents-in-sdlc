---
title: "Ejercicio 2: Añadir funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo de trabajo habitual de un desarrollador consiste en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo se ha pasado por alto, hacer otro cambio... Y la lista continúa.

Aquí es donde entra en juego el modo de agente de Copilot.

El modo de agente de Copilot está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Veamos cómo usar el modo de agente para añadir nuevas funciones al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube están diseñados para ofrecer experiencias distintas. El modo de agente trabaja en tu IDE y permite ciclos rápidos de comentarios e interacción. El agente en la nube funciona como un compañero de programación, de forma asíncrona, como un miembro del equipo con quien interactúas mediante incidencias y solicitudes de cambios.

En este ejercicio aprenderás:

- cómo puede explorar el proyecto el modo de agente de Copilot, identificar archivos pertinentes y realizar cambios coordinados.
- cómo puede implementar nuevas funcionalidades en la interfaz de usuario y la capa de datos.
- cómo revisar los cambios y las pruebas generados por el modo de agente de Copilot antes de combinarlos con el código base.

## Situación

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya añadiste un helper de editoriales. Ahora terminarás el trabajo restante en la capa de datos, la interfaz de usuario y las pruebas con el modo de agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio web de Tailspin Toys para entender sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con una temática para desarrolladores. Permite mostrar juegos y consultar sus detalles. Se trata de una única aplicación Astro que genera las páginas en HTML estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz de usuario del lado del cliente. Los helpers reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se añade mediante un pequeño elemento `<script>` de Astro con ámbito específico que utiliza las API DOM estándar.

### Iniciar el sitio web

Para facilitar la ejecución del sitio web, se proporciona la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Puedes ejecutarla en tu GitHub Codespace siguiendo estos pasos:

1. Vuelve a tu codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre una nueva ventana del terminal en el codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva para este trabajo con los filtros y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el siguiente comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, verás un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si se abre un cuadro de diálogo para pedirte que abras una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando la **x**.

5. Abre el sitio web manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar un enlace a la URL localhost en el terminal, se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado a tu codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Cuando el sitio web esté en ejecución, podrás explorar sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar la lista de trabajo con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará lo que corresponda.

La implementación inicial del sitio web funciona, pero queremos ampliarla con nuevas capacidades. Empecemos revisando la lista de trabajo. Al crear el proyecto a partir de la plantilla, se generó automáticamente una lista de incidencias. Pide a GitHub Copilot que te las muestre.

1. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat. El agente **Agent** planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal y usa herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no están expresando una preferencia por un modelo concreto. Al preparar el taller, usamos Claude Sonnet 4.5 y lo incluimos en las instrucciones para que las sugerencias de código sean relativamente coherentes y la experiencia sea satisfactoria. Sin embargo, como los modelos de lenguaje grandes (LLM) son probabilísticos, es posible que las sugerencias recibidas sean distintas de las indicadas en el taller. Es completamente normal.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede usar un comando MCP diferente, pero debería poder completar la tarea.

3. Pregunta a Copilot por la lista de incidencias enviándole este mensaje:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista de incidencias generada.

Observa que Copilot también ha priorizado los elementos según lo que considera más útil para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, conviene revisar el archivo de instrucciones que usarás para proporcionar contexto a Copilot. Aprovecharás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio web.

1. En tu codespace, ve a `.github/instructions/ui.instructions.md`.
2. Fíjate en las directrices generales para añadir funcionalidad. Incluyen:
   - Una descripción general de la arquitectura.
   - Principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - Enlaces a instrucciones específicas para distintos tipos de archivos, como:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia tanto a otros archivos de instrucciones como a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Esto permite reutilizar contenido, dividir conjuntos de instrucciones complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementar la funcionalidad de filtros

Para completar los filtros hay que realizar al menos tres cambios independientes en la aplicación:

- Añadir o mejorar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- Añadir o actualizar las pruebas del comportamiento de los filtros.
- Actualizar la página de lista de juegos para incorporar la interfaz de los filtros.

Además, hay que ejecutar las pruebas y confirmar que se superan antes de combinar los cambios con el código base. El modo de agente de Copilot puede encargarse de estas tareas. Añadamos la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla de la selección del archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según los requisitos de la incidencia correspondiente de la lista de trabajo:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad. Deberías verlo buscando tanto los helpers de la capa de datos y la interfaz de usuario como las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código base, puede tardar unos minutos. Durante el proceso, puede que aparezcan los botones **Keep** y **Undo** en el editor de código. Cuando Copilot termine, habrá un botón **Keep** o **Undo** para cada cambio; no hace falta seleccionarlos mientras el agente esté trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo en el panel Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de pantalla de una sesión completa con el modo de agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código que generen Copilot u otras herramientas de IA.

10. Vuelve al navegador donde se está ejecutando el sitio web y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo de agente de GitHub Copilot para añadir nuevas funciones al sitio web de Tailspin Toys. Has aprendido:

- cómo puede implementar nuevas funcionalidades en la interfaz de usuario y la capa de datos el modo de agente de GitHub Copilot.
- cómo puede explorar el proyecto el modo de agente de Copilot, identificar archivos pertinentes y realizar cambios coordinados.
- cómo revisar los cambios y las pruebas generados por el modo de agente de Copilot antes de combinarlos con el código base.

Ahora [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de cambios.

### Ejercicio de exploración adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que la implemente en el sitio. Al planificar el código:

- sigue las prácticas recomendadas existentes, incluido el uso de los archivos de instrucciones disponibles.
- piensa cómo quieres implementar la paginación y si quieres permitir que los usuarios seleccionen el tamaño de página o establecerlo de forma fija.
- al redactar el mensaje, proporciona a Copilot las directrices necesarias para crear la implementación que quieres.
- puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. ¡Es lo habitual al trabajar con Copilot!

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
