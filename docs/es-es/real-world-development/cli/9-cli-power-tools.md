---
title: "Lección 9 - Comandos slash de GitHub Copilot CLI"
description: "Explora comandos slash para gestionar el contexto, elegir modelos, compartir sesiones y delegar trabajo opcionalmente al agente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como otras buenas herramientas de CLI, GitHub Copilot CLI incluye muchos comandos slash. Estos comandos ofrecen funciones avanzadas, información interna y opciones de configuración adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Exploremos otros útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usarás `/model` para explorar los modelos disponibles.
- aprenderás cómo `/share` puede exportar o compartir una sesión.
- explorarás comandos opcionales para el trabajo en paralelo, los árboles de trabajo y la delegación al agente en la nube.

## Escenario

Has completado el flujo de trabajo principal de CLI. Ahora explorarás otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y, opcionalmente, delegar trabajo al [agente en la nube de Copilot][about-cloud-agent].

## Explora el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, puedes alcanzar el límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando hace falta; también puedes inspeccionar o compactar el contexto con comandos slash.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y cómo se distribuye el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Es posible que no haya cambiado mucho si la conversación ya era breve.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente cuando se llena la ventana. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` si cambias a una tarea sin relación y quieres empezar una conversación nueva.

## Elige un modelo

Los distintos modelos tienen capacidades diferentes y cada desarrollador puede tener sus preferencias. Copilot CLI permite mostrar los modelos disponibles y elegir uno.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Colaborar en equipo y compartir lo aprendido ayuda a mejorar el uso de las herramientas de inteligencia artificial (IA). El comando `/share` puede exportar una sesión a un archivo Markdown o en lenguaje de marcado de hipertexto (HTML), crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y consulta las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir esta sesión, elige el destino que mejor se adapte a tus necesidades, como `/share file` para crear un archivo Markdown local.
3. Revisa el contenido exportado antes de enviárselo a alguien o publicarlo. Las exportaciones de sesiones pueden incluir indicaciones, respuestas y detalles del proyecto.

Publicar un enlace o gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no tenga previsto compartir.

## Opcional: amplía el trabajo o delégalo

El taller principal ha terminado. Copilot CLI también ofrece comandos para tareas más amplias:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente en la nube de Copilot, que trabajará de forma asíncrona y quizá abra un pull request.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o iniciar trabajo remoto. Empieza con una tarea nueva y bien delimitada y revisa el resultado mediante el flujo habitual. Si quieres profundizar en el trabajo asíncrono de agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos slash de Copilot CLI permiten configurar la herramienta, compartir sesiones y consultar lo que ocurre internamente. En esta lección:

- has usado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- has usado `/model` para explorar los modelos disponibles.
- has aprendido cómo `/share` puede exportar o compartir una sesión.
- has explorado comandos opcionales para el trabajo en paralelo, los árboles de trabajo y la delegación al agente en la nube.

Copilot CLI ofrece muchos más comandos slash y funciones que explorar. Para terminar, [repasarás lo aprendido][next-lesson] y descubrirás los siguientes pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente en la nube de Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
