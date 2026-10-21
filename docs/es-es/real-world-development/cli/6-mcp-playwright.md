---
title: "Lección 6 - Valida la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar la funcionalidad de filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos señalado, programar no consiste únicamente en escribir código. También hay que trabajar con datos y servicios externos, y permitir que Copilot disponga de automatizaciones adicionales. Para ello sirven los servidores MCP: permiten que Copilot CLI vaya más allá de las funciones integradas y acceda a otras herramientas y servicios.

En esta lección:

- entenderás qué es el Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- añadirás el servidor Playwright MCP si aún no está disponible.
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero validar cambios en la interfaz de usuario también requiere interactuar con ella. Quieres que Copilot use el sitio web como lo haría un usuario para automatizar la validación de los cambios y aumentar la confianza en que funcionan como se espera.

## ¿Qué es el Model Context Protocol (MCP)?

El [Model Context Protocol (MCP)][mcp-blog-post] permite a los agentes de inteligencia artificial (IA) comunicarse con herramientas y servicios externos. Gracias a MCP, los agentes pueden acceder a información actualizada y realizar acciones en tu nombre.

Se accede a estas herramientas y recursos mediante un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. Cada servidor MCP ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos son:

- **[Servidor MCP de GitHub][github-mcp]**: permite acceder a las interfaces de programación de aplicaciones (API) para gestionar repositorios, incidencias y pull requests de GitHub.
- **[Servidor Playwright MCP][playwright-mcp-server]**: ofrece funciones de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP. GitHub aloja un [registro de MCP][mcp-registry] para facilitar que se descubran y se contribuya a ellos.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y ten en cuenta las implicaciones de seguridad.

## Añade el servidor Playwright MCP

Si el servidor Playwright MCP aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio web como lo haría un usuario.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos de configuración y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, escribe el siguiente comando para abrir el diálogo de configuración de servidores MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** con <kbd>Enter</kbd> y pulsa <kbd>Tab</kbd>.
5. Pega el siguiente texto en el diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el nuevo servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del diálogo MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Antes comprobaste manualmente que la funcionalidad se comportaba como se esperaba. Ahora usa el servidor Playwright MCP disponible en Copilot para hacer lo mismo.

1. Usa esta indicación para pedir a Copilot que valide la funcionalidad con el servidor Playwright MCP:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Normalmente no hace falta pedir a Copilot que use el servidor MCP, porque suele identificarlo por su cuenta. Aun así, indicarle la herramienta adecuada puede hacer que los resultados sean más coherentes y ahorrar tokens.

2. Observa cómo Copilot enumera los pasos que realiza en el navegador para comprobar la funcionalidad.
3. Lee el informe y asegúrate de que todo se comporta como se espera.

Copilot iniciará el servidor, usará Playwright para interactuar con el sitio web, detendrá el servidor y te presentará un informe.

## Resumen y siguientes pasos

Has usado el servidor Playwright MCP para explorar la funcionalidad en un navegador real desde Copilot CLI. En concreto:

- has aprendido qué es el Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- has añadido el servidor Playwright MCP si aún no estaba disponible.
- has pedido al agente que controle un navegador y explore la funcionalidad de filtrado.

A continuación, [crearás un agente de QA personalizado][next-lesson] que combinará la skill y las herramientas del navegador en un rol especializado.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor Microsoft Playwright MCP][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
