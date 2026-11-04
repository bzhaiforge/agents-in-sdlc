---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, has pedido al agente de Copilot en la nube que se ocupe de tres tareas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas que has asignado al agente, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas de los agentes que tienen solicitudes de incorporación de cambios (PR) abiertas, explorar sus operaciones y orientar una sesión mientras sigue en curso.

En este ejercicio, vas a:

- explorar la página de agentes para supervisar las tareas del agente de Copilot en la nube;
- orientar una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación de un modo de alto contraste, el equipo decide añadir también un modo claro. Como ya se están actualizando los estilos y se está añadiendo un control para alternar el modo, parece razonable incluir esta funcionalidad. Orientarás el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente de Copilot en la nube

Consultarás el estado actual de las tareas que has asignado al agente.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas asignadas a Copilot, entre ellas:
   - actualizar la documentación del código;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio.
3. Selecciona una de las tareas en curso y revisa las operaciones que ha realizado Copilot. Pueden incluir:
   - obtener el código del repositorio;
   - crear el entorno de trabajo de Copilot;
   - configurar servidores MCP;
   - realizar distintos pasos para completar la tarea.

> [!NOTE]
> Los pasos exactos que aparezcan dependerán del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios que aparece a la derecha. Desde ahí puedes consultar la PR y los archivos modificados para supervisar el trabajo.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pedirás a Copilot que añada un control para el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que la sesión indique que ha terminado la configuración y ha empezado a trabajar. Lo notarás cuando aparezcan mensajes parecidos a los de ejemplo.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de pantalla de una tarea del agente en la página de agentes, con el cuadro de diálogo Steer active session while Copilot is working resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que ha recibido el prompt y lo incorpora al trabajo.

## Deja que Copilot trabaje

Copilot continuará con la tarea actualizada e incorporará la nueva solicitud cuando termine el paso en el que estaba trabajando al recibir el mensaje.

El trabajo puede tardar varios minutos. Aprovecha la espera para repasar los conceptos que has explorado.

## Resumen y pasos siguientes

Has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas del agente de Copilot en la nube. Permite consultar las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso para ajustar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- has explorado el centro de control y la página de agentes para supervisar las tareas del agente de Copilot en la nube;
- has redirigido una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot termina el trabajo sobre la accesibilidad, [iterarás sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Siguiente ejercicio: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
