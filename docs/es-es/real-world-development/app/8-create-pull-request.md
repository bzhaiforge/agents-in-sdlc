---
title: "Lección 8 - Crear y combinar la solicitud de incorporación de cambios de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas; después, crea una solicitud de incorporación de cambios y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es hora de revisarlos conjuntamente y abrir una solicitud de incorporación de cambios. Combinaste manualmente la solicitud de incorporación de cambios (PR) de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente, separaríamos la funcionalidad, las actualizaciones de instrucciones, la actualización de la habilidad y el agente QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y todos esos cambios irán en esta PR.

En esta lección, vas a:

- aprender qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- inspeccionar la PR completa de la funcionalidad y las pruebas de verificación;
- autorizar Agent Merge solo después de revisar los cambios y confirmar que la PR se ha combinado.

## Escenario

A lo largo del flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere ahora automatizar el trabajo restante de la PR sin quitar al desarrollador el control sobre la autorización de combinación.

## Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al habilitarlo, la sesión de la aplicación lee la PR y resuelve los impedimentos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y reorganiza la rama sobre la base de destino cuando es necesario. La combinación se realiza en cuanto GitHub lo permite. Agent Merge funciona en segundo plano, sigue activo después de reiniciar la aplicación y se desactiva cuando la PR se combina.

Hasta ahora, has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa tarea, pero todavía necesita tu autorización explícita para editar código y combinar. Revisa las acciones que tiene permitidas y su trabajo antes de concederle permiso para combinar.

## Deja que Agent Merge gestione la PR

Cuando hayas creado y revisado el código, autoriza a Agent Merge a gestionar la PR.

1. Utiliza el selector de agentes para elegir **Default agent**.
2. Selecciona la lista desplegable situada junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción de la lista desplegable no lo inicia.

Se inicia el proceso de Agent Merge. El agente:

- crea la solicitud de incorporación de cambios con un título y una descripción;
- si la sesión se inició desde una incidencia, la incluye como referencia en el cuerpo de la descripción;
- reorganiza la rama sobre la base de destino o resuelve los posibles conflictos de combinación;
- supervisa CI para comprobar que se superan todas las comprobaciones;
- supervisa los comentarios de otros desarrolladores y de la revisión de código de Copilot, y actualiza la PR para resolverlos;
- puede combinar la PR automáticamente cuando todo esté listo.

Permite que Agent Merge también combine la PR cuando se superen todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona la lista desplegable situada junto a **Agent Merge**.
7. Comprueba que **Merge pull request** esté marcado.
8. Pide explícitamente al agente que combine la PR:

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. Sigue el proceso y resuelve los impedimentos que se indiquen. Confirma que el estado de la PR sea **Merged** antes de continuar; tener habilitado el permiso de combinación o un agente en ejecución no confirma que la PR se haya combinado.

> [!IMPORTANT]
> Agent Merge no evita las protecciones del repositorio ni los permisos que falten. Resuelve esos impedimentos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación del código, y ahora el flujo de trabajo de la PR. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- has inspeccionado la PR completa de la funcionalidad y las pruebas de verificación;
- has autorizado Agent Merge solo después de revisar los cambios y has confirmado que la PR se ha combinado.

A continuación, [utilizarás un lienzo existente y crearás un lienzo de clasificación][next-lesson] para explorar una forma más completa de inspeccionar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
