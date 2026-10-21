---
title: "Ejercicio 3 - Prueba tu funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Lección siguiente: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios (PR), comprueba que funciona en el navegador. En lugar de probar la aplicación manualmente, conectarás el **servidor Playwright MCP** y pedirás a Copilot que controle un navegador real para probar la funcionalidad. Después, publicarás la rama y abrirás el PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas de Copilot.
- añadirás el servidor Playwright MCP al espacio de trabajo.
- pedirás a Copilot que lo use para probar manualmente la funcionalidad en un navegador.
- publicarás la rama y abrirás un pull request para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente resulta mucho más útil cuando puede acceder a herramientas fuera del editor. Model Context Protocol (MCP) es el estándar que permite a Copilot comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo de agente y su interacción con el contexto, un modelo de lenguaje grande (LLM) y las herramientas, incluidas las proporcionadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite a los agentes de inteligencia artificial (IA) comunicarse con herramientas y servicios externos. Los agentes pueden usar recursos para consultar información actualizada y herramientas para realizar acciones en tu nombre.

Se accede a estas herramientas y recursos mediante un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. El servidor MCP gestiona la comunicación entre el agente y las herramientas externas, como las interfaces de programación de aplicaciones (API) existentes o las herramientas locales, por ejemplo, los paquetes de NPM. Cada servidor MCP ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos son:

- **[Servidor MCP de GitHub](https://github.com/github/github-mcp-server)**: ofrece API para gestionar repositorios de GitHub. Permite que el agente realice acciones como crear repositorios, actualizar los existentes y gestionar incidencias y pull requests.
- **[Servidor Playwright MCP](https://github.com/microsoft/playwright-mcp)**: ofrece automatización del navegador mediante Playwright. Permite al agente navegar a páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que ofrecen acceso a distintas herramientas y recursos. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar que se descubran y se contribuya a ellos.

> [!CAUTION]
> Por motivos de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar un servidor MCP, revisa su código fuente, verifica quién lo publica y ten en cuenta las implicaciones de seguridad. Usa solo servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el codespace.
2. Deberías ver que ya está configurado un servidor `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

La plantilla del proyecto incluye la entrada `github`; por eso Copilot pudo consultar la lista de incidencias en el ejercicio anterior. Usa el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hace falta instalar nada en local: VS Code se conecta mediante HTTP y te autenticas con GitHub la primera vez que Copilot usa una de sus herramientas.

## Añade el servidor Playwright MCP

Añade un segundo servidor. El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`. El archivo debe quedar así:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en el equipo ejecutando `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin mostrar una ventana, algo necesario en un codespace, donde no hay un escritorio para visualizarla.

> [!NOTE]
> Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, así que normalmente el navegador necesario ya está instalado. Si Copilot informa de que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia y autoriza el servidor

VS Code inicia los servidores MCP **cuando se necesitan**. La primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, aparece la acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para que pueda iniciarse.

Iniciarlo manualmente es opcional. Si omites estos pasos, VS Code iniciará el servidor y te pedirá que confíes en él la primera vez que Copilot necesite sus herramientas en el siguiente paso.

## Prueba la funcionalidad de filtrado

El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador real. En lugar de navegar manualmente por la aplicación, el agente puede abrir una página, navegar por ella, aplicar filtros y leer los resultados para resumir lo que observa. Así puedes confirmar rápidamente que una funcionalidad se comporta como esperas sin salir de la conversación.

El servidor Playwright MCP usa el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. Así, el agente trabaja con elementos estructurados y etiquetados —como botones, enlaces y elementos de lista—, igual que las tecnologías de asistencia. Por tanto, una comprobación funcional también sirve como revisión básica de accesibilidad.

Con el servidor conectado y la aplicación en marcha, pide a Copilot que pruebe la funcionalidad de filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot iniciará un navegador mediante el servidor Playwright MCP, seguirá cada paso y te comunicará lo que haya observado. Compara el resumen con los criterios de aceptación de la incidencia. Si algo no funciona como esperas, haz preguntas de seguimiento o pide a Copilot que corrija el código antes de abrir el pull request.

> [!NOTE]
> Para realizar esta prueba, la aplicación debe estar disponible en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar la indicación. La primera vez que Copilot use el servidor Playwright MCP, quizá tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea un pull request

Después de confirmar que la funcionalidad funciona, publica la rama `filtering-vscode` para abrir un pull request (PR) y pedir al equipo que la revise.

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de commit con el botón **Sparkle**.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para publicar la rama en el repositorio.

## Crea el pull request

Hay varias formas de crear un pull request, por ejemplo, en github.com o con la interfaz de línea de comandos (CLI) de GitHub. Como ya estás trabajando con GitHub Copilot, pídele que cree el PR: puede localizar la incidencia correspondiente y asociarla.

1. Ve al panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** está seleccionado en el menú desplegable de agentes para permitir que Copilot use las herramientas de GitHub.
3. Pide a Copilot que cree el PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si es necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones necesarias. La primera vez que use una herramienta de GitHub, quizá tengas que autenticarte con GitHub. Sigue las indicaciones para autorizarlo.
5. Observa cómo Copilot busca en las incidencias, encuentra la correcta y crea el PR.
6. Selecciona el enlace generado por Copilot para revisar el pull request, pero **no lo fusiones todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas de Copilot.
- has añadido el servidor Playwright MCP al espacio de trabajo.
- lo has usado para probar manualmente la funcionalidad de filtrado en un navegador antes de publicarla.
- has publicado la rama y abierto un pull request para el trabajo de filtrado.

Has usado un servidor MCP para *probar* una funcionalidad, pero MCP solo es una de las formas de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas especializadas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor Microsoft Playwright MCP][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
