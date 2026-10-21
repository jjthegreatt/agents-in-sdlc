---
slug: es-es/real-world-development/cloud
title: "Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent) permite que GitHub Copilot trabaje de forma asíncrona en la nube. Asignas el trabajo en GitHub y el agente en la nube se ocupa de él en segundo plano: explora el repositorio, realiza cambios y abre un pull request mientras tú sigues con otras tareas.

En estos ejercicios añadirás instrucciones personalizadas que seguirá el agente en la nube y, después, le asignarás una incidencia de GitHub para que implemente el trabajo. Revisarás y usarás agentes personalizados para definir su enfoque, supervisarás y orientarás las sesiones desde el panel de agentes y, por último, revisarás sus pull requests e iterarás sobre los resultados.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea tu repositorio y un codespace |
| [1. Instrucciones personalizadas][ex1] | Contexto | Añade instrucciones personalizadas que seguirá el agente en la nube |
| [2. Agente en la nube][ex2] | Agente asíncrono | Asigna incidencias al agente en la nube de GitHub Copilot |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revisa y usa agentes personalizados |
| [4. Gestión de agentes][ex4] | Supervisión | Supervisa y orienta las sesiones de los agentes |
| [5. Iteración][ex5] | Revisión | Revisa los pull requests, itera sobre el trabajo de Copilot y elige los siguientes pasos |

## Requisitos previos

Antes de participar en este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan **Copilot Student, Pro, Pro+, Business o Enterprise** activo
- [ ] El agente en la nube de Copilot habilitado para tu cuenta u organización

> [!TIP]
> ¿No tienes un plan de pago? El alumnado con condición verificada puede obtener GitHub Copilot gratis a través de [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye el agente, Model Context Protocol (MCP), la revisión de código y las funciones de Copilot CLI que se usan en este taller, así que podrás completar todos los entornos con él.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> El agente en la nube está disponible en **Copilot Student, Pro, Pro+, Business y Enterprise**: en todos los planes excepto Copilot Free. En Copilot Business o Enterprise, un administrador debe habilitarlo para la organización.

> [!NOTE]
> Este entorno no cubre el Model Context Protocol (MCP). Para explorar el uso de servidores MCP con Copilot, consulta el [entorno CLI](../cli/) o el [entorno de VS Code](../vscode/).

## Empieza

**[Empieza por el ejercicio 0: Requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
