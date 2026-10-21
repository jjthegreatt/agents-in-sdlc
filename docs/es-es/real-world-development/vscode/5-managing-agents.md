---
title: "Ejercicio 5 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo de agente de GitHub Copilot, este trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en el equipo en tiempo real, Copilot Chat en VS Code muestra las llamadas a herramientas y las ediciones de archivos a medida que se producen. Puedes revisar cada diferencia en el chat, aceptar o rechazar los cambios por separado y orientar la conversación con indicaciones de seguimiento para modificar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones de archivo propuestas.
- orientarás al agente durante la sesión para que añada un selector del modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro para quienes consideren demasiado oscuro el tema predeterminado. En lugar de empezar una conversación desde cero, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y los cambios que propone. Cada edición aparece en el chat junto con una diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas. Verás lecturas y ediciones de archivos y los comandos de terminal cuya ejecución solicite.
3. Cuando proponga una edición, aparecerá la diferencia en el chat:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos que el agente ha modificado durante la sesión.

> [!NOTE]
> Si una llamada a herramienta solicita permiso, por ejemplo, para ejecutar un comando del terminal, el agente se detendrá y mostrará la opción **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios.

## Orienta al agente para que añada el modo claro

Ahora que está implementado el modo de alto contraste, amplía la misma conversación para añadir un selector del modo claro. Como continuarás en la misma sesión, el agente conserva el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilos y el patrón de almacenamiento local.

1. En el campo de entrada de Copilot Chat, envía esta indicación de seguimiento:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente continuará el trabajo, explorará los cambios realizados y propondrá las modificaciones para el modo claro.
3. Revisa cada edición nueva a medida que se aplique, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** en cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, si intenta reorganizar los controles de una forma que no te gusta—, detenlo con el botón **Stop** junto al campo de entrada y envía otra indicación para aclarar el requisito.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Usa indicaciones de seguimiento para pedir ajustes («simplifica las hojas de estilo en cascada (CSS); usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también atajos de teclado para ambos controles»).

## Revisa los archivos modificados

Antes de confirmar los cambios, revisa todos los archivos que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y examina las diferencias. Confirma que se aplican las directrices de accesibilidad del agente personalizado: atributos de las Aplicaciones de Internet enriquecidas accesibles (ARIA), navegación con teclado, lenguaje de marcado de hipertexto (HTML) semántico y almacenamiento local de las preferencias.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado las ediciones de archivos, aceptado o rechazado cambios y enviado indicaciones de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- has supervisado el trabajo del agente en tiempo real.
- has aceptado y rechazado ediciones de archivos individuales.
- has orientado una sesión en curso con indicaciones de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en un pull request y revisarás los cambios que el agente personalizado local haya generado durante este taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Lección siguiente: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
