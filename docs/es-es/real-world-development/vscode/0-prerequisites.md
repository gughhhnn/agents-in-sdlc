---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, tienes que prepararlo todo. Crearás una copia propia del repositorio Tailspin Toys, iniciarás un [codespace][codespaces] para trabajar y confirmarás que GitHub Copilot Chat está funcionando en el editor.

## Configurar el repositorio del laboratorio

Para crear una copia del repositorio en la que trabajarás, crearás una instancia a partir de la [plantilla][template-repository]. La nueva instancia contendrá todos los archivos necesarios para el laboratorio y la utilizarás mientras completas los ejercicios.

1. En una nueva ventana del navegador, ve a la [plantilla de Tailspin Toys][tailspin-template].
2. Crea una copia propia del repositorio seleccionando el botón **Use this template** en la página del repositorio del laboratorio. Después, selecciona **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si vas a completar el taller en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, puedes crear el nuevo repositorio en una organización a la que tengas acceso con GitHub Copilot.

    ![Configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**nombre-de-organización-o-usuario/nombre-del-repositorio**), ya que la necesitarás más adelante en el laboratorio.

> [!NOTE]
> **La lista de trabajo está preparada**
>
> Al crear el repositorio a partir de la plantilla, se generará automáticamente una lista de trabajo de incidencias de GitHub. Trabajarás con estas incidencias durante el taller; no tienes que crear ninguna.

## Crear un codespace

A continuación, usarás un codespace para completar los ejercicios del laboratorio.

[GitHub Codespaces][codespaces] es un entorno de desarrollo basado en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Ofrece un IDE completo con compatibilidad con varios lenguajes de programación, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

    ![Selección del botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un codespace.

    ![Creación de un codespace](../../../_images/ex0-create-codespace.png)

La creación del codespace tardará varios minutos, aunque seguirá siendo mucho más rápida que instalar todos los servicios manualmente. Mientras tanto, puedes explorar otras funciones de GitHub Copilot, que veremos a continuación.

> [!CAUTION]
> Volverás al codespace en un ejercicio futuro. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está diseñado para ejecutarse en un codespace o en un [contenedor de desarrollo][dev-containers] local. Ambos entornos incluyen todos los requisitos previos necesarios para que la experiencia sea fluida. Si prefieres ejecutarlo localmente, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite. VS Code creará el mismo contenedor de desarrollo que utiliza el codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usar GitHub Copilot Chat y el modo de agente

Para acceder al modo de agente de GitHub Copilot Chat, debes tener instalada la extensión GitHub Copilot Chat en el IDE. Si estás usando un GitHub Codespace, ya debería estar instalada.

> [!TIP]
> Si no tienes instalada la extensión GitHub Copilot Chat, puedes [instalarla desde Visual Studio Code Marketplace][copilot-chat-extension]. También puedes abrir la vista Extensiones en Visual Studio Code, buscar **GitHub Copilot Chat** y seleccionar **Install**.

Una vez instalada la extensión, puede que tengas que autenticarte con tu cuenta de GitHub para habilitarla.

1. Vuelve a tu codespace.
2. Si no ves Copilot Chat en el lado derecho del editor, selecciona el icono **Copilot Chat** en la parte superior del codespace.
3. Escribe un mensaje como «Hello world» en la ventana de Copilot Chat y pulsa Entrar. Así se activará Copilot Chat.
4. Si no te has autenticado, se te pedirá que inicies sesión en tu cuenta de GitHub. Sigue las instrucciones para autenticarte.

    ![Ejemplo de una solicitud de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Después de autenticarte, verás la ventana de Copilot Chat.

## Resumen

¡Enhorabuena! Has creado una copia del repositorio del laboratorio. También has iniciado el proceso de creación del codespace que utilizarás cuando empieces a escribir código.

## Siguiente paso

Empecemos a poner Copilot a trabajar. Continúa con el [Ejercicio 1: Instrucciones personalizadas][next-lesson], donde enseñarás a Copilot las convenciones del proyecto.

## Recursos

- [Introducción a GitHub Codespaces][codespaces]
- [Creación de un repositorio a partir de una plantilla][template-repository]
- [Primeros pasos con Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
