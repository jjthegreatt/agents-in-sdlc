---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Lección siguiente: Modo de agente →][next-lesson]

El contexto es fundamental cuando se trabaja con inteligencia artificial (IA) generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información adicional, conviene asegurarse de que pueda acceder a ella. Hay varias formas de compartir contexto con Copilot. Una de las más importantes son los [archivos de instrucciones][instruction-files], que proporcionan directrices sobre la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos asociados a rutas.
- generarás la primera parte de los datos para el filtrado —un helper de editores— con las instrucciones actuales.
- añadirás un estándar general nuevo al repositorio en `.github/copilot-instructions.md`.
- volverás a enviar la misma indicación y observarás cómo el código generado adopta el nuevo estándar.
- confirmarás en `main` la actualización de instrucciones y la primera parte de la implementación para que Copilot pueda usar las directrices actualizadas en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede no cumplir todos los estándares indicados. Copilot no es determinista. El objetivo del ejercicio es observar la tendencia de los cambios de comportamiento después de actualizar las instrucciones, no obtener exactamente el mismo código carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo, Tailspin Toys cuenta con directrices y requisitos para las prácticas de programación. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación de TypeScript (TSDoc).
- La parte superior de cada archivo debe incluir un bloque de comentarios que describa su función.

Con los archivos de instrucciones te asegurarás de que Copilot disponga de la información necesaria para realizar las tareas conforme a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para ayudarle a entender tu estilo de programación y tus requisitos. Permiten especificar convenciones de programación, bibliotecas e incluso el tipo de comentarios que se deben incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot para **todas** las solicitudes del repositorio. Debe contener información del proyecto relevante para la mayoría de las interacciones, como la pila tecnológica, una descripción del proyecto, las prácticas recomendadas y otras directrices generales.
- Los archivos `.github/instructions/*.instructions.md` se pueden crear para tareas o tipos de archivo concretos. Puedes usarlos para indicar directrices específicas para lenguajes como TypeScript o Astro, o para tareas como crear componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para sugerencias de edición siguiente.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot [admite otros formatos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

El alcance de este taller no incluye explicar en detalle cómo crear archivos de instrucciones. Los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén `copilot-instructions.md` centrado en directrices generales del proyecto, como una descripción del trabajo, la estructura del proyecto y los estándares globales de programación.
- Usa archivos `*.instructions.md` para tipos de archivo concretos —como pruebas unitarias, componentes Astro o la capa de datos— o para tareas específicas.
- Escribe en lenguaje natural. Expresa las directrices con claridad e incluye ejemplos de código correctos e incorrectos.

No hay una única forma correcta de crear archivos de instrucciones ni de usar la IA. La experimentación te ayudará a encontrar lo que funciona para tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían contar con una colección sólida de archivos de instrucciones. Al explorar los del proyecto, quizá veas archivos para tareas como [actualizar la interfaz de usuario][ui-instructions] o trabajar con [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece una opción distinta, como **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente lo indicará cuando sea pertinente.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones del repositorio: incluye un archivo principal `copilot-instructions.md` y otros `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y observa la descripción breve del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Estas indicaciones se aplican a todas las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: establece un patrón glob, relativo a la raíz del repositorio, que determina a qué archivos se aplican las instrucciones. En este caso, coincide con archivos de pruebas TypeScript, por ejemplo, los que siguen el patrón `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos amplios de instrucciones en archivos reutilizables más pequeños y enlazar ejemplos que Copilot pueda seguir al generar código. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige documentación dentro del código. En los pasos siguientes añadirás reglas para incluir comentarios TSDoc y encabezados de comentarios de archivo.

## Crea una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para estos cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará lo que corresponda.

Para observar el efecto de las instrucciones personalizadas, primero envía una indicación con el archivo de instrucciones actual. Más adelante lo actualizarás y volverás a enviar la misma indicación para comparar el resultado.

1. Cierra las pestañas del editor que tengas abiertas de ejercicios anteriores para que Copilot solo use el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para que Copilot sepa dónde debe ir el helper.
3. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda modificar archivos.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía esta indicación:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios al código; a menudo modificará tanto el archivo del helper como sus pruebas.
6. Observa que el helper propuesto es una función tipada cuyo primer argumento es un cliente `db` y que devuelve una matriz tipada de editores. Esto sigue las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y un encabezado de comentario para el archivo.

> [!CAUTION]
> Como Copilot ofrece resultados probabilísticos, quizá añada comentarios TSDoc aunque no se lo pidas. No pasa nada: el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añade un estándar nuevo al repositorio

Como se ha indicado, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Añade allí los estándares de programación del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que describe los estándares de programación, pero todavía no incluye reglas para documentar el código.
3. Añade estas líneas Markdown justo después de los estándares existentes para indicar que Copilot debe incluir encabezados de comentarios de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, las instrucciones pueden estar en el nivel del repositorio (`.github/copilot-instructions.md`) o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios de documentación que acabas de añadir.

## Vuelve a enviar la indicación y observa el cambio

Repite la misma indicación en una conversación nueva para que Copilot cargue las instrucciones actualizadas.

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía la **misma indicación**:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto empieza ahora con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta incluye ahora un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has guiado a Copilot para que siga un estándar nuevo y lo aplique a código real sobre el que se trabajará en el siguiente ejercicio.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones son recursos del repositorio y se gestionan como cualquier otro archivo mediante el control de versiones. Confirma los cambios y publica la rama.

1. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Confirma que están presentes las instrucciones actualizadas y los cambios del helper:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y todos los archivos que Copilot haya modificado para crear la base del helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra cambios adicionales relacionados (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama en el repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y fusiona un pull request

La rama ya está publicada. Ahora crea un pull request asociado a la incidencia sobre los estándares de documentación de la lista de tareas. Copilot puede localizar la incidencia, crear un PR que la cierre y fusionarlo mediante las herramientas de GitHub que ya están conectadas al proyecto.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub en tu nombre. La plantilla del proyecto ya configura las herramientas de GitHub en el espacio de trabajo, por lo que no hace falta instalar nada. La primera vez que Copilot use una, VS Code quizá te pida que inicies sesión en GitHub. Sigue las indicaciones para autorizarlo. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de las instrucciones y cree un PR desde la rama actual, describiendo tanto la actualización de las instrucciones como la base del helper de editores:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot empezará a buscar la incidencia y crear el PR.
5. Cuando Copilot te pida **Allow** para realizar acciones de GitHub, revisa el comando y selecciona **Allow** si corresponde.
6. Una vez creado el PR, pídele a Copilot que lo fusione y vuelva a `main` en local:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida **Allow** para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** si corresponde.

¡Has creado y fusionado un pull request con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo obtiene Copilot el contexto de los archivos de instrucciones del proyecto y, después, has usado Copilot Chat en VS Code para:

- explorar cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos asociados a rutas.
- enviar una indicación de generación de código y observar el resultado con las instrucciones *actuales*.
- añadir un estándar general nuevo a `.github/copilot-instructions.md`.
- volver a enviar la misma indicación y observar cómo el código propuesto adopta el estándar nuevo.
- confirmar la actualización de instrucciones y la base del helper en `main` para que el siguiente ejercicio pueda usarlas.

A continuación, aplicarás esas instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad en distintas partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
