---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de inteligencia artificial (IA) especializados para tareas o ámbitos concretos del flujo de desarrollo. Puedes definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio y proporcionarles instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos que les ayuden a realizar ciertos tipos de trabajo. Los equipos pueden plasmar sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las [Pautas de Accesibilidad para el Contenido Web (WCAG)][wcag], un agente de seguridad que siga prácticas de programación segura o un agente de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen mediante archivos Markdown en `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo contiene frontmatter YAML con, al menos, los campos `name` y `description`, seguido de una indicación Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Comparación entre agentes personalizados y skills de agente

Los agentes personalizados y las [skills de agente][agent-skills-concept] se parecen en algunos aspectos. Ambos se definen principalmente con archivos Markdown e indican a la IA cómo realizar tareas. La diferencia más sencilla es que un **agente personalizado** realiza el trabajo y las **skills** son herramientas.

Los agentes personalizados disponen de su propia ventana de contexto y están diseñados para coordinar skills e incluso otros agentes mientras trabajan. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad; para ello, podría invocar skills como una que gestione pull requests o una que ejecute y administre pruebas.

> [!NOTE]
> No hay una única forma correcta de crear agentes personalizados. Como con cualquier uso de IA, prueba y modifica el enfoque para adaptarlo al entorno y a las situaciones.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a garantizar que todo el mundo pueda usar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Comentarios recientes indican que algunas personas tienen dificultades para leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para abordar este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es fundamental, así que quieres implementar esta función cuanto antes con ayuda de un agente personalizado.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- usarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo guiará a Copilot.

Vuelve a tu codespace, abre un terminal y crea una rama nueva desde `main` para el trabajo de accesibilidad. Así mantendrás separado el PR de filtrado del ejercicio 3.

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código en lenguaje de marcado de hipertexto (HTML), hojas de estilo en cascada (CSS) y JavaScript.
   - errores y problemas habituales.

## Usa el agente personalizado en Copilot Chat

VS Code muestra en el menú desplegable de agentes de Copilot Chat todos los agentes personalizados definidos en `.github/agents`. Puedes seleccionar uno para limitar la sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará lo que corresponda.

Selecciona el agente de accesibilidad y pídele que implemente el modo de alto contraste.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** si aún no está seleccionado.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el menú desplegable de agentes en la parte inferior de la vista Chat. Muestra el agente activo, que por defecto es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía esta indicación al agente de accesibilidad:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y una diferencia que podrás revisar.
6. A medida que se apliquen cambios, el indicador **Files changed** se actualizará para mostrar los archivos que haya modificado el agente.

> [!NOTE]
> El proceso puede tardar unos minutos. Copilot realizará cambios en el repositorio y editará archivos existentes, como componentes Astro y archivos CSS, además de las pruebas relacionadas.

En el siguiente ejercicio revisarás y orientarás el trabajo en curso.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados para tareas y ámbitos concretos. Permiten plasmar los conocimientos y estándares del equipo en agentes reutilizables que orientan a Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo usar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson], revisarás los cambios en curso y añadirás un selector para el modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Prueba tu funcionalidad con el servidor Playwright MCP][previous-lesson] | [Lección siguiente: Supervisión y gestión de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
