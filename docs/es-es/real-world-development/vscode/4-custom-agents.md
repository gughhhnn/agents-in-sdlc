---
title: "Ejercicio 4: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados y adaptados a tareas o ámbitos concretos del flujo de trabajo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, prácticas recomendadas, patrones de programación y conocimientos especializados que le permitan realizar ciertos tipos de trabajo de forma más eficaz. Los equipos pueden plasmar sus conocimientos en agentes reutilizables: un agente de accesibilidad que aplique el cumplimiento de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones de prueba coherentes.

Los agentes personalizados se definen en archivos Markdown en la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML que contiene, como mínimo, `name` y `description`, seguido de un mensaje de Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Comparación entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se solapan en algunos aspectos. Ambos se definen principalmente mediante archivos Markdown y explican a una IA cómo realizar operaciones. La distinción más clara es que un **agente personalizado** es el trabajador y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades (e incluso otros agentes) durante el trabajo. En este laboratorio, el agente personalizado de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad. Como parte de su trabajo, podría utilizar habilidades, por ejemplo, una para el flujo de trabajo de las solicitudes de cambios u otra para ejecutar y gestionar pruebas.

> [!NOTE]
> No hay una única manera correcta de redactar un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar lo que mejor funcione en tus entornos y situaciones.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Situación

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes han indicado que a algunas personas les cuesta leer el tema oscuro actual debido al contraste insuficiente entre el texto y el fondo. Para abordar este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que pueda activarse y desactivarse.

La accesibilidad es fundamental, así que queremos implementarlo lo antes posible. Usarás un agente personalizado para generar la funcionalidad.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- usarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisemos su contenido para entender cómo orientará a Copilot.

Vuelve a tu codespace y abre el terminal. Cámbiate a una rama nueva basada en `main` para trabajar en la accesibilidad (mantendrás separada la PR de filtros del ejercicio 3):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en el frontmatter YAML y los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Lee las secciones siguientes, que destacan:
   - Las responsabilidades principales al generar código para un sitio web accesible.
   - Las prácticas recomendadas de accesibilidad.
   - Ejemplos de código HTML, CSS y JavaScript.
   - Errores y problemas habituales.

## Usar el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el menú desplegable de agentes de la parte inferior de la vista Copilot Chat. Puedes seleccionar un agente personalizado para limitar una sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará lo que corresponda.

1. Si aún no está seleccionado, elige **Agent** en el menú desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el menú desplegable de agentes en la parte inferior de la vista de chat. Muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía este mensaje al agente de accesibilidad:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat empezará a trabajar: explorará el código base, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y una diferencia que podrás revisar.
6. A medida que se apliquen los cambios, el indicador **Files changed** se actualizará para mostrar los archivos modificados por el agente.

> [!NOTE]
> El proceso puede tardar unos minutos. Copilot está realizando cambios reales en el repositorio: editará archivos existentes, como los componentes Astro, CSS y pruebas relacionadas.

En el siguiente ejercicio revisarás y orientarás este trabajo mientras está en curso.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados y adaptados a tareas y ámbitos específicos. Con ellos, puedes plasmar la experiencia y los estándares del equipo en agentes reutilizables que orientan a Copilot para realizar ciertos tipos de trabajo de forma más eficaz.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo usar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson]: revisarás los cambios a medida que se realizan y añadirás un interruptor de modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Creación de agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar tu funcionalidad con el servidor Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
