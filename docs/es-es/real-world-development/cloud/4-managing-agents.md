---
title: "Ejercicio 4 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que realizara tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidad. Aunque el agente trabaja de forma autónoma y asíncrona, sigue siendo importante poder supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este panel puedes ver las tareas de los agentes que tienen pull requests (PR) abiertos, explorar las operaciones realizadas e incluso orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo se ha dado cuenta de que también sería conveniente añadir un modo claro. Como ya se están actualizando los estilos del sitio y se está añadiendo la opción de alternar modos, parece lógico incorporar esa función. Orientarás el trabajo del agente para que añada el modo claro además del de alto contraste.

## Revisa las tareas del agente en la nube de Copilot

Consulta el estado de las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar interfaces de programación de aplicaciones (API) para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las operaciones realizadas por Copilot. Entre ellas puede haber:
   - descargar el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores del Model Context Protocol (MCP).
   - realizar distintas tareas para completar el trabajo asignado.

> [!NOTE]
> Los pasos que aparecen dependerán del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de pull request que aparece a la derecha. Permite consultar el PR y los archivos modificados para supervisar el trabajo.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión que hace referencia al modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado actual del trabajo.

   ![Sesión de accesibilidad en el panel de agentes](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Verás mensajes que confirman que se encuentra en esa fase.
3. En el diálogo **Steer active session while Copilot is working**, introduce esta indicación:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarea del agente en la nube en la página de agentes con el diálogo Steer active session while Copilot is working resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la indicación.
5. Observa cómo Copilot confirma que la ha recibido y la incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot continuará con la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

Como antes, el proceso tardará varios minutos. Puedes aprovechar ese tiempo para repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro para supervisar y guiar las tareas del agente en la nube de GitHub Copilot. Desde este panel puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar su alcance o proporcionar directrices adicionales.

Has explorado estos conceptos:

- has explorado el panel de control y la página de agentes para supervisar las tareas del agente en la nube.
- has redirigido una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot completa el trabajo de accesibilidad, puedes [iterar sobre los pull requests que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Lección siguiente: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
