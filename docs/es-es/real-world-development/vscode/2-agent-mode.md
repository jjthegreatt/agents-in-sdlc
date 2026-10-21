---
title: "Ejercicio 2 - Añade funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios más sencillos de una aplicación suelen requerir actualizar varios archivos y realizar tareas como ejecutar pruebas. El flujo de trabajo habitual de un desarrollador consiste en localizar todos los archivos necesarios, hacer los cambios, ejecutar pruebas, depurar, averiguar qué archivo falta, realizar otra actualización... Y así sucesivamente.

Para eso sirve el modo de agente de Copilot.

El modo de agente de Copilot está diseñado para trabajar con más autonomía desde el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura existente del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo usar el modo de agente para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube están diseñados para experiencias diferentes. El modo de agente trabaja en el IDE, lo que permite recibir comentarios e interactuar con rapidez. El agente en la nube trabaja de forma asíncrona como un miembro del equipo y se comunica contigo mediante incidencias y pull requests.

En este ejercicio aprenderás a:

- usar el modo de agente de Copilot para explorar el proyecto, identificar archivos pertinentes y coordinar cambios.
- usar el modo de agente de GitHub Copilot para implementar funcionalidades en la interfaz de usuario y en la capa de datos.
- revisar los cambios y las pruebas que genera el modo de agente de Copilot antes de fusionarlos en el código.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior añadiste un helper de editores; ahora completarás con el modo de agente de Copilot el resto del trabajo en la capa de datos, la interfaz y las pruebas.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para entender sus funciones actuales.

El sitio es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite listar juegos y mostrar sus detalles. Es una única aplicación Astro que genera páginas en lenguaje de marcado de hipertexto (HTML) estáticas durante la compilación. Las páginas consultan una base de datos SQLite local directamente desde su frontmatter mediante Drizzle, un mapeador objeto-relacional (ORM); no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un marco de interfaz del lado del cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se añade con un pequeño `<script>` de Astro con ámbito limitado que usa las API estándar del Modelo de Objetos del Documento (DOM).

### Inicia el sitio web

El proyecto incluye una tarea `npm run dev` que inicia el único servidor de desarrollo de Astro. Ejecútala en GitHub Codespaces siguiendo estos pasos.

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo en el codespace pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web con este comando:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, verás un mensaje con la URL parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un diálogo que te pide abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro, `http://localhost:4321`, en el terminal.

> [!NOTE]
> En un codespace, al seleccionar un enlace a una URL localhost en el terminal de Codespaces, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al codespace, donde ahora se ejecuta el servidor web.

### Explora el sitio web

Cuando el sitio esté en marcha, explora sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explora la lista de tareas con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará lo que corresponda.

La primera versión del sitio funciona, pero queremos añadir funciones nuevas. Empieza por revisar la lista de tareas. Al crear el proyecto a partir de la plantilla, se generó automáticamente una lista de incidencias. Pide a GitHub Copilot que te las muestre.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente puede planificar e implementar cambios de forma autónoma en varios archivos, ejecutar comandos del terminal e invocar herramientas.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no expresan ninguna preferencia por un modelo u otro. Al crear el taller, usamos Claude Sonnet 4.5 y lo indicamos para que las sugerencias de código sean relativamente coherentes y la experiencia sea adecuada. Sin embargo, como los modelos de lenguaje grandes (LLM) son probabilísticos, es normal que las sugerencias varíen respecto a las indicadas en el taller.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje grandes (LLM), Copilot quizá use otro comando del Model Context Protocol (MCP), pero debería poder completar la tarea.

3. Envía esta indicación a Copilot para consultar las incidencias pendientes:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que muestra todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado las incidencias según su criterio sobre cuáles serán más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo que proporciona contexto para la tarea. Usarás el archivo de instrucciones de la [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funcionalidad, entre ellas:
   - una descripción de la arquitectura.
   - principios para diseñar componentes, realizar pruebas y garantizar la accesibilidad.
   - enlaces a archivos de instrucciones específicos para distintos tipos de archivo, incluidos:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar instrucciones, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementa la funcionalidad de filtrado

Para completar el filtrado, tendrás que realizar al menos tres cambios distintos en la aplicación:

- añadir o perfeccionar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de fusionar los cambios. El modo de agente de Copilot puede encargarse de estas tareas.

1. Continúa en la conversación actual con Copilot o inicia otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia correspondiente de la lista de tareas:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad, como los helpers de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos, según la complejidad de la tarea y del código. Durante el trabajo, quizá aparezcan los botones **Keep** y **Undo** en el editor. Cuando Copilot termine, tendrás la opción **Keep** o **Undo** para todos los cambios; no hace falta que los selecciones mientras el agente sigue trabajando.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Diálogo de Copilot Chat que pide confirmar la ejecución de las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. El proceso puede incluir pausas e incluso pruebas fallidas. Copilot alterna entre generar código y ejecutar pruebas hasta terminar la tarea y no detectar errores.

   ![Sesión completa de Copilot Chat con el modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de inteligencia artificial (IA).

10. Vuelve al navegador donde está abierto el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has explorado cómo usar el modo de agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys. Has aprendido a:

- usar el modo de agente de Copilot para explorar el proyecto, identificar archivos pertinentes y coordinar cambios.
- usar el modo de agente de GitHub Copilot para implementar funcionalidades en la interfaz de usuario y en la capa de datos.
- revisar los cambios y las pruebas generados antes de fusionarlos en el código.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás un pull request.

### Ejercicio adicional: implementa la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que la añada al sitio. Ten en cuenta estas consideraciones:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones.
- decide cómo implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- incluye en la indicación la información que Copilot necesita para implementar la solución que quieres.
- quizá tengas que iterar con GitHub Copilot y pedirle cambios o proporcionarle contexto. Es normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
