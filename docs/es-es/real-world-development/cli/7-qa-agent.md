---
title: "Lección 7 - Crea y usa un agente de control de calidad (QA)"
description: "Crea un agente personalizado que combine los requisitos de la incidencia, la skill quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la skill `quality-checks` para ejecutar comprobaciones automatizadas y el servidor Playwright del Model Context Protocol (MCP) para observar la experiencia de filtrado en un navegador. Ahora combinarás esas capacidades en un agente personalizado con un proceso de control de calidad (QA) definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, skills y herramientas MCP.
- crearás e inspeccionarás un perfil de control de calidad reutilizable.
- seleccionarás el agente de QA y revisarás sus conclusiones conforme a la incidencia de filtrado.

## Escenario

Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de las pruebas y el comportamiento en el navegador antes de abrir un pull request (PR). Un agente personalizado puede coordinar el proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot que se define en un perfil Markdown. El perfil describe su finalidad, sus instrucciones y las herramientas disponibles. En este taller definirás un rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has usado tienen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La skill `quality-checks` agrupa comprobaciones repetibles. Playwright MCP proporciona herramientas para el navegador. El perfil de QA indica a Copilot cómo usar esas capacidades para evaluar los requisitos e informar de los resultados. No sustituye a las demás herramientas ni requiere otra conversación.

## Crea el perfil de QA

Antes de abrir el PR de la funcionalidad, pide a Copilot que cree un perfil de QA reutilizable. El perfil definirá las comprobaciones que realizará el agente y los límites que debe respetar.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el nuevo agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecciona el perfil

Antes de usar el agente nuevo, revisa su perfil para confirmar que Copilot ha recogido el flujo de QA y sus límites. Así evitarás que un agente incompleto o demasiado permisivo cambie una funcionalidad que solo quieres verificar.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero permite asignar al agente un nombre claro para mostrar.
3. Lee las instrucciones del perfil y confirma que QA parte de los requisitos, sigue las instrucciones del repositorio, ejecuta la skill `quality-checks` y usa Playwright MCP.
4. Confirma que QA presenta pruebas que respaldan sus conclusiones, pide permiso antes de modificar código de implementación y no confirma cambios ni abre pull requests.
5. Si al perfil le falta alguna responsabilidad o algún límite, pide al agente predeterminado que lo revise antes de continuar.

## Ejecuta QA para la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Retoma la conversación de filtrado después de crear el perfil y selecciona QA para que pueda usar la incidencia y las decisiones de planificación que ya están en el contexto.

1. Activa el agente con esta indicación:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, es posible que no aparezca en la lista de agentes disponibles. El comando anterior lo activará igualmente.

2. Pide al agente de QA que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. Espera a que el agente de QA complete el trabajo.
4. Lee el informe que genere.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable y revisado su trabajo. En esta lección:

- has explorado cómo trabaja un agente personalizado con instrucciones, skills y herramientas MCP.
- has creado e inspeccionado un perfil de control de calidad reutilizable.
- has seleccionado el agente de QA y revisado sus conclusiones conforme a la incidencia de filtrado.

Ya tienes la implementación, la actualización de la skill, el perfil de QA, las pruebas y el informe de verificación listos para revisar. A continuación, [los reunirás en un PR de funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
