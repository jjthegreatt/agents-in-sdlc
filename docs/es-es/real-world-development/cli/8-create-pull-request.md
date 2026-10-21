---
title: "Lección 8 - Crea y fusiona el PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la skill, el perfil de control de calidad (QA) y las pruebas; después, crea un PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones, la actualización de la skill, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Ahora toca revisarlos juntos y abrir un pull request. Tú mismo fusionaste el pull request (PR) de las valoraciones por estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente dividiríamos la funcionalidad, las actualizaciones de instrucciones, la actualización de la skill y el agente de QA en varios PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sola conversación y rama, y lo incluirás en este PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una fusión.
- inspeccionarás todos los cambios de la funcionalidad y las pruebas de verificación.
- crearás el PR del filtrado.
- habilitarás Agent Merge solo después de revisar el cambio y confirmar que el PR se ha fusionado.

## Escenario

Durante el flujo de trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante del PR y mantener bajo el control del desarrollador la autorización de la fusión.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para integrar un pull request. Cuando lo habilitas, Copilot resuelve los impedimentos del PR —por ejemplo, corrige las comprobaciones de integración continua (CI) fallidas, responde a los comentarios de revisión y vuelve a aplicar la base cuando hace falta— y, después, habilita la fusión automática de GitHub cuando el repositorio lo permite.

Hasta ahora, tú mismo has seleccionado **Merge pull request**. Agent Merge puede encargarse de ese trabajo. Revisa el cambio y confirma que está listo antes de habilitar Agent Merge.

## Usa Agent Merge para gestionar el PR

Una vez creado el código, revísalo, crea el PR y deja que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el diálogo del agente escribiendo `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea un PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua del PR. Cuando todo termine correctamente, lo fusionará.
7. Comprueba que Copilot muestre un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo indicado y completa manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y ahora gestionar el pull request. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una fusión.
- has inspeccionado todos los cambios de la funcionalidad y las pruebas de verificación.
- has creado el PR del filtrado.
- has habilitado Agent Merge solo después de revisar el cambio y has confirmado que el PR se ha fusionado.

A continuación, [explorarás otros comandos slash útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Recursos

- [Gestionar pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
