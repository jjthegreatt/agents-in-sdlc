---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para implementar el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto. Sin embargo, los cambios más amplios requieren un proceso más sólido. GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo de una organización y ayudar a crear lo adecuado de la forma correcta. Esta es la primera de varias lecciones en las que seguirás un proceso de desarrollo habitual con agentes: partirás de una incidencia para generar una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como se espera y, por último, la fusionarás correctamente en el proyecto.

> [!NOTE]
> Continuarás el flujo de trabajo de la funcionalidad con la misma conversación y la misma rama. Normalmente usarías ramas o solicitudes de incorporación de cambios (PR) diferentes para los distintos tipos de archivo, pero tomaremos un atajo para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una nueva conversación de Copilot desde una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás manualmente la funcionalidad en el navegador a través del puerto reenviado.

Mientras continúas con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la skill `quality-checks` existente, añadirás validación mediante Model Context Protocol (MCP), crearás un agente de control de calidad (QA) y abrirás el PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan filtrar los juegos por categoría y editor. La incidencia de la lista de tareas describe la funcionalidad, pero hay que acordar detalles como la combinación de categorías antes de empezar a programar. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Introducir agentes de programación con inteligencia artificial (IA) en el flujo de desarrollo no cambia los principios fundamentales. De hecho, adquieren aún más importancia. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Empezar con una incidencia registrada que describa lo que hay que hacer.
2. Crear un plan de lo que se debe desarrollar.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear un pull request (PR).
7. Cuando se haya revisado el código y se hayan superado las comprobaciones de integración continua, fusionar el código.

> [!NOTE]
> Los detalles concretos variarán según el equipo y la organización, pero la mayoría de los procesos seguirán una variante del flujo anterior.

Al seguir este enfoque habitual, te aseguras de que el código generado por IA cumpla los requisitos y pase por el mismo proceso de validación que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el nivel de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas junto con el agente. El agente sugiere cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empieza desde la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que el PR de las valoraciones por estrellas está fusionado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia a la indicación y volver a la pestaña **Session**.

Observa que la indicación empieza por `#7` o un número parecido. El prefijo `#` permite incluir en el contexto una incidencia o un pull request de GitHub.

## Planifica la funcionalidad de filtrado

La planificación permite acordar el enfoque antes de pedir a Copilot que implemente una funcionalidad o tarea. Es recomendable dedicar tiempo a planificar los cambios complejos. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador bajo la indicación muestra **Plan**.
2. Después de la referencia a la incidencia añadida en el paso anterior, escribe esta indicación:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque.

3. Es posible que Copilot te pregunte cómo debe funcionar el filtrado. Responde según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que modifique las partes del plan que no coincidan con tus decisiones.

## Aprueba Autopilot

Una vez escrito y revisado el plan, es hora de implementarlo. Deja que Copilot trabaje con el modo Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que está resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión instalada.
2. Confirma que el indicador bajo la indicación muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación inmediatamente, así que revisa el plan antes. Si Copilot informa de dependencias ausentes o de un conflicto de puerto, resuelve el problema de configuración antes de dar las comprobaciones por completadas.

## Revisa y verifica la implementación

Revisa el código generado y ejecuta el sitio para comprobar que todo funciona correctamente, igual que harías antes de fusionar cualquier otro cambio.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones tomadas durante la planificación.
4. Cuando termines de revisar el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los fallos.

## Prueba la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio en el navegador a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, por editor y las combinaciones acordadas en el plan.
4. Confirma que el restablecimiento y el comportamiento cuando no hay resultados coinciden con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una nueva conversación de Copilot desde una incidencia de GitHub.
- has definido los requisitos en el modo Plan.
- has implementado la funcionalidad con el modo Autopilot.
- has revisado el código.
- has validado manualmente la funcionalidad en el navegador a través del puerto reenviado.

A continuación, profundizarás en cómo se genera el código y te asegurarás de que siga las prácticas documentadas mediante el [uso de instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
