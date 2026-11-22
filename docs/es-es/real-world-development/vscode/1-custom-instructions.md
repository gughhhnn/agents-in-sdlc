---
title: "Ejercicio 1: Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, o hay información previa que Copilot debería conocer, conviene asegurarse de que tenga acceso a ese contexto. Hay varias formas de compartir información específica con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que permiten orientar la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos de rutas.
- generarás la primera parte de la capa de datos para los filtros (un helper de editoriales) con las instrucciones *actuales*.
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.
- volverás a ejecutar el mismo mensaje y observarás cómo el código generado adopta el nuevo estándar.
- confirmarás la actualización de las instrucciones y la parte de los filtros en `main` para que Copilot pueda utilizar las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no cumplir algunos de los estándares que hayas definido. Copilot no es determinista. El objetivo del ejercicio es observar la *tendencia* de cambio en el comportamiento tras actualizar las instrucciones, no obtener exactamente el mismo resultado carácter por carácter.

## Archivos de instrucciones

### Situación

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe tener un modo oscuro y un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su función.

Con los archivos de instrucciones, te asegurarás de que Copilot tenga la información necesaria para realizar las tareas de acuerdo con las prácticas indicadas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo y tus requisitos de programación. Son una herramienta eficaz para orientar sus sugerencias y fragmentos de código. Puedes especificar las convenciones de código, las bibliotecas e incluso los tipos de comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción general de lo que se está creando, las prácticas recomendadas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` pueden crearse para tareas o tipos de archivos específicos. Puedes utilizarlos para proporcionar directrices para lenguajes concretos (como TypeScript o Astro) o para tareas como crear un componente de interfaz de usuario o un nuevo conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto los archivos de instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también [admite archivos de instrucciones que siguen otros estándares][custom-instructions-support], incluidos AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Una conversación completa sobre cómo crear archivos de instrucciones queda fuera del alcance del taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. A grandes rasgos:

- Mantén las instrucciones de `copilot-instructions.md` centradas en las directrices del proyecto: una descripción de lo que se está creando, la estructura del proyecto y los estándares globales de código.
- Usa archivos `*.instructions.md` para proporcionar instrucciones específicas para tipos de archivos (pruebas unitarias, componentes Astro, capa de datos) o tareas concretas.
- Escribe en lenguaje natural. Redacta directrices claras y proporciona ejemplos de cómo debería ser el código y de cómo no debería ser.

No hay una única manera correcta de crear archivos de instrucciones, igual que no hay una única manera de usar la IA. La experimentación te ayudará a encontrar lo que mejor funcione para el proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener una colección completa de archivos de instrucciones. Al explorar los archivos de este proyecto, puede que veas instrucciones para muchos tipos de tareas, incluidas las [actualizaciones de la interfaz de usuario][ui-instructions] y los archivos [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente al entorno en el que estés trabajando lo indicará cuando sea pertinente.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio lleno de archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizados del proyecto

Dedica un momento a leer los archivos de instrucciones de este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Estas instrucciones se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y echa un vistazo. Observa que hay instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón global (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de prueba TypeScript, por ejemplo, uno que siga el patrón `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot pueda seguir al generar código. (Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de código del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentarios de los archivos.

## Crear una rama para los cambios

Sigamos algunas prácticas recomendadas y creemos una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará lo que corresponda.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un mensaje con el archivo de instrucciones actual. Más adelante lo actualizarás y volverás a enviar el mismo mensaje para comparar el resultado.

1. Cierra las pestañas abiertas del editor de los ejercicios anteriores para que Copilot solo tenga el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para que Copilot sepa dónde debe estar el helper.
3. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat para que Copilot pueda aplicar cambios a los archivos.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este mensaje:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica actualizaciones al código, a menudo tanto al archivo helper como a sus pruebas.
6. Fíjate en que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentarios del archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios de documentación aunque no se lo hayas pedido. No pasa nada: el objetivo es observar la *mejora de coherencia* después de actualizar las instrucciones.

## Añadir un nuevo estándar del repositorio

Como ya hemos indicado, `.github/copilot-instructions.md` está pensado para proporcionar información sobre el proyecto. Vamos a documentar los estándares de código del repositorio para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que documenta los estándares de código del proyecto, pero aún no contiene una regla para la documentación en el código. Por eso el helper generado no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown justo después de los estándares existentes para indicar a Copilot que añada encabezados de comentarios a los archivos y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, los archivos de instrucciones pueden estar en el nivel del repositorio (`.github/copilot-instructions.md`) para ofrecer directrices globales o ser archivos `*.instructions.md` para lenguajes, tipos de archivos o tareas específicos. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla sobre los comentarios de documentación que acabas de añadir.

## Volver a enviar el mensaje y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar el contexto.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía el **mismo mensaje** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar del proyecto y lo aplique a código real que utilizarás en el siguiente ejercicio.

## Confirmar las instrucciones y subir la rama

Los archivos de instrucciones son recursos del repositorio y se gestionan con el mismo sistema de control de versiones que los demás archivos. Confirmemos los cambios y subamos la rama.

1. Abre una nueva ventana del terminal pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Desde el terminal, confirma que los cambios en las instrucciones y el helper están presentes:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de las instrucciones y todos los archivos que Copilot haya modificado para crear la base del helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados, como pruebas, añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Desde el terminal, sube la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de cambios

Ya has subido la rama, así que vamos a crear una solicitud de cambios y vincularla a la incidencia sobre los estándares de documentación de la lista de trabajo. Podrías hacerlo manualmente, pero Copilot puede encargarse mediante las herramientas de GitHub que ya están conectadas al proyecto. Pídele que busque la incidencia, cree una PR que la cierre y, después, la combine.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actuará en GitHub en tu nombre. La plantilla del proyecto ya conecta las herramientas de GitHub con tu espacio de trabajo, así que no hay que configurar nada. La primera vez que Copilot use una herramienta, VS Code puede pedirte que inicies sesión en GitHub. Sigue las indicaciones para autorizarlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en tu codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y que cree una PR desde la rama actual, indicando tanto las actualizaciones de instrucciones como la nueva base del helper de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot empezará a buscar la incidencia y crear la PR.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** si procede.
6. Cuando se cree la PR, pídele a Copilot que la combine y vuelva a `main` con este mensaje:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** si procede.

¡Ya has creado y combinado una solicitud de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot incorpora el contexto de los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un mensaje para generar código y observar lo que produce Copilot con las instrucciones *existentes*.
- añadir un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.
- volver a enviar el mismo mensaje y observar cómo el código propuesto adopta el nuevo estándar.
- confirmar la actualización de las instrucciones y la base del helper en `main` para que el siguiente ejercicio pueda partir de ahí.

A continuación, pondrás en práctica esas instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una nueva funcionalidad en todo el código base.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
