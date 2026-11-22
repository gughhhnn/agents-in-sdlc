---
title: "Lección 12: Implementar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de cambios y verificar que Space Quiz está publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica Space Quiz para que todo el mundo pueda acceder a él con GitHub Pages. Copilot puede preparar el flujo de trabajo y guiar la implementación, pero tú eres responsable de revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure una implementación con GitHub Pages.
- revisarás el flujo de trabajo y la solicitud de cambios antes de combinarlos.
- seguirás el proceso de implementación y verificarás el sitio publicado.

## Situación

Space Quiz ya está en un repositorio de GitHub, pero hace falta una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages lo hace accesible mediante una URL pública sin añadir un servidor ni una dependencia de entorno de ejecución.

## Pedir a Copilot que prepare la implementación

> [!WARNING]
> Crear una solicitud de cambios puede confirmar y subir código. Revisa la configuración de aprobaciones y el repositorio de destino antes de enviar este mensaje. Si seleccionas **Approve all**, no esperes una confirmación adicional.

En la sesión de la aplicación Copilot para `space-quiz`, envía este mensaje:

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## Revisar el flujo de trabajo y la solicitud de cambios

1. Confirma que el README solo se ha añadido si el repositorio aún no tenía uno.
2. Revisa los cambios de la solicitud, incluidos los pasos de compilación e implementación del flujo de trabajo, los activadores y los permisos.
3. Confirma que el flujo implementa desde la rama predeterminada, no desde una solicitud de cambios sin combinar.
4. Pide las correcciones necesarias y revisa la diferencia actualizada. Espera a que se superen las comprobaciones obligatorias de la solicitud antes de autorizar la combinación.

## Autorizar la combinación y la implementación

> [!WARNING]
> El mensaje siguiente autoriza al agente a combinar la solicitud revisada, cambiar la configuración de Pages del repositorio y publicar el sitio. Envíalo solo después de terminar la revisión y confirmar el repositorio y la solicitud. Según la configuración de aprobaciones, puede que estas acciones se realicen sin pedir confirmación adicional.

Sustituye `<PR_NUMBER>` por el número de la solicitud de cambios que has revisado y envía este mensaje por separado:

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de tres etapas: una solicitud de cambios combinada de space-quiz; una ejecución correcta de GitHub Actions «Deploy website» con los pasos «Build website», «Upload artifact» y «Deploy to Pages»; y Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verificar la implementación

1. Sigue la ejecución de GitHub Actions hasta que se complete correctamente.
2. Abre la URL proporcionada y confirma que se carga Space Quiz.

Si el agente no puede habilitar Pages o combinar la solicitud de cambios, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que la implementación haya finalizado correctamente y el sitio publicado se cargue.

## Resumen y siguientes pasos

Space Quiz ya está disponible como sitio publicado y has verificado la implementación, en lugar de depender del informe del agente. Continúa con la [Lección 13: Revisión y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
