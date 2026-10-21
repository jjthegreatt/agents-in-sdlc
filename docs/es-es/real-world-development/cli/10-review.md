---
title: "Lección 10 - Recapitulación y siguientes pasos"
description: "Repasa el flujo de trabajo con Copilot CLI, los dos pull requests, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI a lo largo de un flujo de trabajo continuo en Tailspin Toys. En concreto:

- has preparado un Codespace, instalado Copilot CLI, explorado el proyecto y localizado la incidencia de filtrado preparada.
- has añadido valoraciones por estrellas, revisado el resultado en un navegador reenviado y fusionado manualmente tu primer pull request (PR).
- has partido de la incidencia de filtrado, definido el enfoque en el modo Plan, implementado en Autopilot y revisado en Interactive.
- has guiado al agente con instrucciones personalizadas y, después, has personalizado la skill `quality-checks` existente y la has usado para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has añadido el servidor Playwright del Model Context Protocol (MCP) y lo has usado para explorar el filtrado en un navegador real.
- has creado y seleccionado un agente de control de calidad (QA) personalizado para evaluar los requisitos, la cobertura, los resultados de la skill y las pruebas del navegador.
- has revisado todos los cambios del filtrado y autorizado Agent Merge para el PR.
- has explorado comandos slash para gestionar el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Lo que has publicado

El taller tiene dos hitos de PR, cada uno en su propia rama basada en `main` actualizado:

1. **Valoraciones por estrellas:** mostrar el `starRating` existente y un estado explícito cuando no haya valoración en las tarjetas de juegos.
2. **Filtrado y flujo de calidad:** implementar el filtrado, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe de `quality-checks`, crear un perfil de QA e incluir las pruebas correspondientes.

Desde la planificación del filtrado hasta la creación de su PR, has usado la misma conversación y rama. Para agilizar el taller, hemos reunido todo ese trabajo en un solo PR.

## Diferentes tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, inspeccionando el navegador y explorando el navegador con Copilot mediante MCP. La skill `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos y las incluyó en el nuevo formato de informe. QA reunió esos resultados con una revisión de los requisitos y la cobertura de las pruebas antes del PR.

Las pruebas añadidas deben cubrir lagunas reales; una ejecución de QA que no necesite pruebas nuevas también puede ser correcta. Revisa el código y las pruebas antes de autorizar la fusión, y actualiza las pruebas afectadas cuando haya cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller has actualizado instrucciones, personalizado una skill, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y adáptalas a las necesidades del equipo. Las instrucciones establecen estándares, las skills describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para considerar el enfoque antes de empezar, **Interactive** para mantener el control en cambios acotados y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para las ediciones rutinarias y uno más capaz para el trabajo complejo.

El contexto es tan importante como la infraestructura. Describir claramente *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más opciones

Ya has recorrido el flujo de trabajo principal. Puedes explorar otras funciones de CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para hablar de un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Siguientes pasos

La mejor manera de mejorar con cualquier herramienta es seguir usándola. Úsala para código de producción, proyectos personales o esa pequeña aplicación que llevas años pensando en crear. Comparte lo que aprendas con tu equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar pull requests con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
