---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidad. Le has pedido que documente el código, cree una sección de juegos relacionados para que el equipo de diseño pueda evaluar la experiencia e implemente funciones de accesibilidad, incluidos los modos de alto contraste y claro. Ahora revisarás los cambios de código y, si hace falta, darás indicaciones a Copilot para mejorar el resultado.

### Escenario

Como ya hemos señalado varias veces, la inteligencia artificial (IA) generativa no cambia los principios fundamentales del diseño de software ni de DevOps. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Por tanto, revisaremos las propuestas de GitHub Copilot para documentar el código, añadir los juegos relacionados y mejorar la accesibilidad antes de habilitar la revisión del resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

El agente en la nube de Copilot realiza tareas de forma asíncrona y sin supervisión directa. Para mantener la seguridad, se aplican varias restricciones:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usará para el código.
- El agente en la nube se ejecuta en GitHub Actions, en un entorno independiente y efímero.
- Un usuario debe aprobar los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores del Model Context Protocol (MCP).

## Revisa la documentación generada

Empieza por el primer pull request (PR) que generó el agente en la nube de GitHub Copilot: el que añade documentación al código. Para ello, usarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al abrir el PR, quizá veas un aviso de que el cortafuegos ha bloqueado GitHub Copilot. Es **lo esperado**: el acceso a recursos externos, incluidas las llamadas a servidores MCP externos, está limitado de forma predeterminada. Si quieres, puedes [personalizar o deshabilitar el cortafuegos del agente en la nube de Copilot][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre el PR titulado, por ejemplo, **Add missing documentation** o con un título más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, el PR tendrá la marca **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo aprendido.

4. Cuando el PR esté listo, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que contiene los nuevos comentarios TSDoc y otros cambios de documentación. Los cambios concretos pueden variar.

   Busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado los cambios y te parezcan correctos, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en cola en la sección de comprobaciones del pull request. Si todo va bien, las comprobaciones del proyecto de la aplicación Astro finalizarán correctamente. El proceso puede tardar unos minutos.

## Solicita cambios a GitHub Copilot

Trabajar con Copilot en un pull request no es una interacción unidireccional. Puedes mencionar a Copilot en comentarios del PR, como harías con otros miembros del equipo, o en comentarios insertados en el código. Copilot verá los comentarios e iniciará otra sesión para responder. Como los resultados no son deterministas, no podemos darte un texto exacto. Puedes pedirle, por ejemplo:

- que añada al principio de cada archivo de código un encabezado con una breve descripción.
- que añada comentarios TSDoc a los archivos TypeScript y Astro.
- que cree un README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier usuario. Usa una de las ideas anteriores u otra sugerencia sobre la documentación del código.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una nueva sesión para aplicar los cambios.
3. Puedes seleccionar **Back to pull request** para volver al pull request.

   ![Volver al pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot haya terminado, debería aparecer un commit nuevo en el pull request.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, convierte el PR en un PR listo para revisión y fusiónalo en la rama principal.

![Marcar el PR como listo para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Ahora revisa el PR que Copilot creó para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre el PR titulado, por ejemplo, **Show related games on the game details page** o con un título más descriptivo.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios y te parezcan correctos, vuelve a **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en cola en la sección de comprobaciones del pull request. Si todo va bien, las comprobaciones del proyecto de la aplicación Astro finalizarán correctamente. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente la funcionalidad de juegos relacionados. Ve al codespace, abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes iterar con el agente en la nube para solicitar cambios. Por ejemplo, puedes pedirle que ajuste el número de juegos relacionados o que añada encabezados de comentarios y comentarios TSDoc. Recuerda que esta incidencia se asignó **antes** de actualizar las instrucciones personalizadas. Para solicitar cambios, añade un comentario nuevo en la pestaña **Conversation**; Copilot lo verá e iniciará otra sesión.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones de accesibilidad implementadas con el agente personalizado. Este PR debería incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro que solicitaste desde el panel de agentes en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre el PR titulado, por ejemplo, **Add high contrast mode to website** o con un título más descriptivo.

> [!NOTE]
> Si Copilot aún está trabajando, el pull request tendrá la marca **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los componentes de alternancia entre los modos.
   - el uso del almacenamiento local para guardar las preferencias.
   - los cambios de hojas de estilo en cascada (CSS) o estilos de los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con el teclado.
   - el código JavaScript o TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado los cambios y te parezcan correctos, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en cola en la sección de comprobaciones del pull request. Si todo va bien, las comprobaciones del proyecto de la aplicación Astro finalizarán correctamente. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Ve al codespace, abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama que creó Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Después, inicia la aplicación y prueba los selectores de los modos de alto contraste y claro en el navegador. Confirma que funcionan y que las preferencias se conservan al volver a cargar las páginas.

Observa cómo el agente personalizado de accesibilidad ha ayudado a guiar la implementación conforme a las prácticas recomendadas. Si detectas problemas de accesibilidad o aspectos que se pueden mejorar, menciona a **@copilot** en un comentario para solicitar cambios, igual que hiciste en los PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta más útil cuando puedes asignarle tareas reales de la lista de trabajo y ocuparte de otras cosas mientras tanto. Para acostumbrarte, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- Crear un formulario para expresar interés en la página de detalles del juego.
- Implementar la paginación en la página de lista de juegos.
- Añadir validación de entrada y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el entorno del agente en la nube. A lo largo de estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas del repositorio** para comprobar su efecto posterior en el resultado del agente en la nube.
- **Has asignado incidencias al agente en la nube de Copilot** y has observado cómo configuraba el entorno, planificaba y ejecutaba el trabajo de forma asíncrona.
- **Has creado y usado un agente personalizado** para la accesibilidad, con modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como panel de control** para supervisar y orientar la sesión de accesibilidad mientras estaba en curso.
- **Has revisado los pull requests del agente en la nube e iterado sobre ellos**, mencionando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Revisión y siguientes pasos

Has completado el entorno del agente en la nube. Si quieres seguir explorando, los otros entornos complementan lo que has practicado aquí:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP desde el IDE.
- 💻 **[Entorno CLI](../../cli/)**: publica dos cambios revisados desde el terminal con los modos Plan y Autopilot, skills de agente, un agente QA personalizado, Playwright MCP, Agent Merge y una integración opcional con el agente en la nube mediante `/delegate`.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
