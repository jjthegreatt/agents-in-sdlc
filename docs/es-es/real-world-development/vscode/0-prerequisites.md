---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara todo lo necesario. Crearás tu propia copia del repositorio Tailspin Toys, pondrás en marcha un [codespace][codespaces] y confirmarás que GitHub Copilot Chat funciona en el editor.

## Configura el repositorio del taller

Para crear una copia del repositorio en la que trabajarás, crearás una instancia a partir de la [plantilla][template-repository]. La nueva instancia incluirá todos los archivos necesarios para el taller.

1. En una ventana nueva del navegador, ve a la [plantilla de Tailspin Toys][tailspin-template].
2. Crea tu copia seleccionando **Use this template** en la página del repositorio del taller. Después, selecciona **Create a new repository**.

   ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si haces el taller en un evento dirigido por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, puedes crear el repositorio en una organización donde tengas acceso a GitHub Copilot.

   ![Configuración del repositorio de la plantilla](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**); la necesitarás más adelante en el taller.

> [!NOTE]
> **La lista de tareas ya está preparada**
>
> Al crear el repositorio a partir de la plantilla, se crea automáticamente una lista de incidencias de GitHub. Trabajarás con ellas durante todo el taller; no hace falta que crees incidencias.

## Crea un codespace

A continuación, usarás un codespace para completar los ejercicios.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un entorno de desarrollo integrado con compatibilidad para varios lenguajes de programación, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

   ![Botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un codespace.

   ![Creación de un codespace](../../../_images/ex0-create-codespace.png)

La creación del codespace tardará unos minutos, aunque será más rápida que instalar manualmente todos los servicios. Mientras tanto, puedes explorar otras funciones de GitHub Copilot, que veremos a continuación.

> [!CAUTION]
> Volverás al codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está diseñado para ejecutarse en un codespace o en un [contenedor de desarrollo][dev-containers] local. Ambos entornos incluyen los requisitos previos necesarios. Si prefieres trabajar en local, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando aparezca el aviso. VS Code creará el mismo contenedor de desarrollo que usa el codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usa GitHub Copilot Chat y el modo de agente

Para usar el modo de agente de GitHub Copilot Chat, necesitas tener la extensión GitHub Copilot Chat instalada en el IDE. Debería estar instalada si estás usando GitHub Codespaces.

> [!TIP]
> Si no tienes instalada la extensión GitHub Copilot Chat, puedes [instalarla desde Visual Studio Code Marketplace][copilot-chat-extension]. También puedes abrir la vista **Extensions** en Visual Studio Code, buscar **GitHub Copilot Chat** y seleccionar **Install**.

Después de instalar la extensión, quizá tengas que autenticarte con tu cuenta de GitHub para habilitarla.

1. Vuelve a tu codespace.
2. Si Copilot Chat no aparece en el lado derecho del editor, selecciona el icono **Copilot Chat** en la parte superior del codespace.
3. Escribe un mensaje como «Hello world» en la ventana de Copilot Chat y pulsa <kbd>Enter</kbd>. Así se iniciará Copilot Chat.
4. Si no estás autenticado, se te pedirá que inicies sesión en tu cuenta de GitHub. Sigue las instrucciones para autenticarte.

   ![Ejemplo de solicitud de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Después de autenticarte, debería aparecer la ventana de Copilot Chat.

## Resumen

¡Enhorabuena! Has creado una copia del repositorio del taller y has empezado a crear el codespace que usarás cuando comiences a programar.

## Siguiente paso

Empieza a trabajar con Copilot. Continúa con el [ejercicio 1: Instrucciones personalizadas][next-lesson], donde enseñarás a Copilot las convenciones del proyecto.

## Recursos

- [Información general sobre GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Guía de inicio de Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
