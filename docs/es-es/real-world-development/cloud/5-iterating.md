---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidad. Le has pedido que añada documentación al código, cree una función de juegos relacionados sobre la que pueda iterar el equipo de diseño e implemente funciones de accesibilidad con interruptores de modo de alto contraste y claro. Vamos a explorar los cambios y, si hace falta, daremos indicaciones a Copilot para mejorar el resultado.

### Situación

Como ya hemos indicado varias veces, los fundamentos del diseño de software y DevOps no cambian al incorporar IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Con esto en mente, revisemos las sugerencias de GitHub Copilot para crear la documentación, la función de juegos relacionados y las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot realiza tareas de forma asíncrona y sin supervisión, se han establecido determinadas restricciones de seguridad para que el entorno siga siendo seguro:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para el código.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero para trabajar.
- Los flujos de trabajo de GitHub Actions requieren la aprobación de una persona antes de ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empecemos explorando la primera solicitud de cambios (PR) generada por el agente en la nube de GitHub Copilot: la que añade documentación al código. Utilizarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, puede que aparezca una advertencia que indique que el firewall ha bloqueado GitHub Copilot. Es **normal**, ya que Copilot tiene acceso limitado de forma predeterminada a los recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube de Copilot][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes de cambios.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation** u otro título más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar o reflexionar sobre lo que has aprendido.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluirá los nuevos comentarios TSDoc y otra documentación. Los cambios exactos variarán.

   Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas pautas proceden de los archivos de instrucciones personalizadas que revisaste al principio del Ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar unos minutos.

## Solicitar cambios a GitHub Copilot

La colaboración con Copilot en una solicitud de cambios no es un proceso unidireccional. También puedes mencionarlo en comentarios de la PR, como harías con cualquier otro miembro del equipo, o añadir comentarios en línea al código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos sugerir un mensaje exacto. Por ejemplo, podrías pedir a Copilot que:

- añada encabezados breves al principio de cada archivo de código para describir su función.
- añada comentarios TSDoc a los archivos TypeScript y Astro.
- cree un archivo README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Usa una de las ideas anteriores u otra sugerencia para la documentación que quieras incluir en el código base.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Verás que inicia una sesión nueva para realizar las actualizaciones.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud.

   ![Volver a la solicitud de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, debería aparecer una confirmación nueva en la solicitud.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés satisfecho. Cuando lo estés, convierte la solicitud en borrador a lista para revisión y combínala en la rama principal.

![Convertir una PR en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la función de juegos relacionados

Volvamos a la solicitud que Copilot generó para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Show related games on the game details page** u otro título más descriptivo.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador de que algunos flujos de trabajo están pendientes de aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar unos minutos.
9. **Opcional:** también puedes cambiar a esta rama en tu codespace para probar manualmente la función de juegos relacionados. Ve al codespace, abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes trabajar de forma iterativa con el agente en la nube y pedirle actualizaciones. Por ejemplo, puedes pedirle que cambie el número de juegos relacionados o que añada encabezados de comentarios y comentarios TSDoc (recuerda que asignaste esta tarea **antes** de actualizar las instrucciones personalizadas). Como con las demás PR, puedes añadir una solicitud como nuevo comentario en la pestaña **Conversation**. Copilot la verá e iniciará una sesión nueva.

## Revisar las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. La PR debería incluir el modo de alto contraste que asignaste en el Ejercicio 3 y el modo claro que solicitaste desde el panel de control en el Ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Add high contrast mode to website** u otro título más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - Los componentes de interfaz de los interruptores de modo.
   - El uso del almacenamiento local para conservar las preferencias del usuario.
   - Los cambios de CSS o estilo para los modos de alto contraste y claro.
   - Los atributos de accesibilidad (etiquetas ARIA, navegación por teclado, etc.).
   - El código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu codespace para probar manualmente las funciones de accesibilidad. Ve al codespace, abre el terminal y ejecuta los comandos siguientes. Sustituye `<branch-name>` por el nombre de la rama que haya creado Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los interruptores de modo de alto contraste y claro en el navegador. Confirma que funcionan como se espera y que la preferencia se conserva al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad orientó a Copilot para implementar estas funciones siguiendo las prácticas recomendadas de accesibilidad. Si detectas problemas o mejoras posibles, puedes mencionar a **@copilot** en un comentario y pedirle que actualice los cambios, como hiciste con las PR anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube funciona mejor cuando le asignas elementos reales de la lista de trabajo y puedes ocuparte de otras cosas. Para adquirir esa costumbre, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- Crea un formulario de interés para patrocinadores en la página de detalles del juego.
- Implementa la paginación en la página de la lista de juegos.
- Añade validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. A lo largo de estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas que incluye este repositorio** para observar su efecto en el trabajo del agente.
- **Has asignado incidencias al agente en la nube de Copilot** y has visto cómo configuraba el entorno, planificaba y trabajaba de forma asíncrona.
- **Has creado y utilizado un agente personalizado** de accesibilidad para añadir interruptores de modo de alto contraste y claro.
- **Has usado la página de agentes de Copilot como panel de control** para supervisar y orientar la sesión de accesibilidad mientras estaba en curso.
- **Has revisado las solicitudes de cambios del agente en la nube e iterado sobre ellas** al mencionar a `@copilot` para solicitar cambios y aprobar flujos de trabajo.

## Revisión y siguientes pasos

Has completado el itinerario del agente en la nube. Si quieres seguir explorando, los demás itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y un puente opcional `/delegate` con el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignación de incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]
- [Configuración del firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Supervisar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
