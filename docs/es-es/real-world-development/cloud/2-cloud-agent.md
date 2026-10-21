---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que pocas organizaciones no tengan dificultades con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en la lista de tareas porque no había tiempo para implementarlas. El agente en la nube de GitHub Copilot está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea un pull request (PR) en borrador para que lo revise un desarrollador. Así se pueden delegar tareas tediosas, acelerar el desarrollo y dejar que los desarrolladores se centren en cuestiones de mayor alcance.

En este ejercicio explorarás cómo usar el agente en la nube de Copilot para:

- personalizar el entorno en el que genera código.
- garantizar que las operaciones se realicen de forma segura.
- comprender la importancia de delimitar bien las incidencias.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica pendiente. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado poco adecuado: en realidad, no hay ninguna. Como primer paso, el equipo quiere añadir comentarios de documentación de TypeScript (TSDoc) a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la exploración de juegos. Quiere que la página de detalles de cada juego muestre otros juegos relacionados —títulos de la misma categoría— para que la gente pueda seguir explorando. Por ahora no hace falta una implementación pulida; basta con algo que el equipo pueda usar para probar la experiencia de usuario. Aunque esto bloquea el trabajo, hay otras incidencias con mayor prioridad.

Estos son ejemplos de tareas que suelen quedar relegadas y que pueden asignarse al agente en la nube de Copilot. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, revisar el resultado para comprobar que todo es correcto.

## Introducción al agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Después de recibirla, Copilot crea un pull request en borrador para registrar su progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o cuando haya terminado. Cuando la solución esté lista para revisarla, Copilot te mencionará en el pull request.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el trabajo de análisis. Incluso las tareas que parecen sencillas pueden resultar complejas al examinarlas en detalle, así que [delimita con cuidado las tareas del agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un compañero de programación con inteligencia artificial (IA): trabaja por etapas, aprende, experimenta y adapta el enfoque. La IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores has visto cómo guían a Copilot las instrucciones personalizadas. Si ya has completado el ejercicio práctico de instrucciones personalizadas, has visto `.github/copilot-instructions.md` en acción; si no, puedes echarle un vistazo ahora. Antes de asignar trabajo al agente en la nube, revisa los archivos de instrucciones incluidos en este repositorio. No los modifiques todavía: así podrás reconocer su efecto más adelante.

Abre los archivos de instrucciones siguientes en la interfaz web de GitHub o en un codespace que ya tengas en marcha:

1. Abre `.github/copilot-instructions.md` y revisa la sección **Code standards**, en particular las expectativas relativas a las convenciones de TypeScript y los comentarios TSDoc.
2. Abre `.github/instructions/unit-tests.instructions.md` y observa el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation` al agente en la nube en la siguiente sección, busca en el pull request resultante comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Proceden de estos archivos de instrucciones. Volveremos a hablar de ello al revisar los PR en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube

La creación de código suele requerir un entorno específico y la ejecución de scripts de configuración, sea quien sea el responsable. Lo mismo se aplica al asignar tareas a Copilot, que trabaja de forma similar a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] para configurar su entorno de trabajo. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Así tendrá acceso a las herramientas y dependencias de desarrollo necesarias. Este flujo ya está configurado para el taller. Instala Node.js y las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos de ejemplo de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar estos pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Asigna los permisos mínimos necesarios para estos pasos. Copilot recibirá su propio token para realizar sus operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo para instalar dependencias, necesitarán el permiso `contents: read`. Si no lo clonan, Copilot lo hará automáticamente cuando terminen los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (toda la aplicación usa Astro y Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga la base de datos SQLite local con datos de ejemplo para las compilaciones y las pruebas.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero hay algunos detalles importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en el pull request.
- Observa el activador `workflow_dispatch`, que permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona sin tener que esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo entiende la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica suele quedar pendiente, reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot puede ayudar a crear documentación, por lo que esta incidencia es adecuada para asignársela al agente en la nube. Trabajará en segundo plano para añadir la documentación necesaria. En un ejercicio posterior revisarás el resultado.

1. En una pestaña nueva del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el diálogo de creación de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Introduce esta descripción:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el diálogo de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre el pull request nuevo, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si no aparece, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista de tareas de Copilot puede tardar varios minutos en aparecer en el PR. Copilot está configurando su entorno mediante el flujo de trabajo anterior, analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista y las tareas que va a realizar.
14. Desplázate por la cronología del pull request. Deberías ver un aviso de que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la ventana para ver el indicador actualizado.

16. Desplázate por la sesión en curso y observa cómo Copilot resuelve el problema: explora el código, analiza el estado, se detiene a pensar y elegir un plan, y crea código.

El proceso puede tardar varios minutos. Una ventaja principal del agente en la nube de Copilot es que puede trabajar de forma asíncrona mientras tú te ocupas de otras tareas. Aprovecharemos esa función: asignaremos otra tarea al agente y, mientras trabaja, nos centraremos en añadir funcionalidad a la aplicación.

## Añade una sección de juegos relacionados a los detalles del juego

Una de las ventajas del agente en la nube es que permite dividir el trabajo: tú puedes centrarte en unas tareas mientras el agente se ocupa de otras. Aunque añadir una sección de juegos relacionados quizá no lleve mucho tiempo, podrías dedicar ese tiempo a otras tareas. Asígnasela al agente en la nube de Copilot.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. Introduce esta descripción:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el diálogo de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, verás el emoji 👀 en el primer comentario de la incidencia, que indica que Copilot ya está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. Como el agente en la nube trabaja de forma parecida a un ingeniero de software, no hace falta supervisarlo continuamente; basta con revisar el resultado cuando termine. Ahora nos centraremos en crear y usar agentes personalizados.

## Resumen y siguientes pasos

En este ejercicio has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignar incidencias a Copilot para que trabaje de forma asíncrona en deuda técnica, nuevas funcionalidades o migraciones entre marcos de trabajo.

Has explorado estos conceptos:

- personalizar el entorno en el que se genera código.
- garantizar que las operaciones se realicen de forma segura.
- comprender la importancia de delimitar bien las incidencias.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja en segundo plano, puedes crear y usar agentes personalizados. El [agente en la nube también puede usar servidores del Model Context Protocol (MCP)][cloud-agent-mcp] y dispone de instrucciones personalizadas, que has explorado en ejercicios anteriores.

## Recursos

- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Lección siguiente: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
