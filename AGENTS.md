# AGENTS.md — Protocolo de trabajo para agentes de IA

Este documento define cómo debe comportarse cualquier agente de IA que trabaje en este repositorio:
el `.github` de la organización Vela Bank. Aquí no hay producto; hay gobierno compartido. El objetivo
es que el agente **no tenga rienda suelta**: primero propone, después ejecuta y al final rinde cuentas.

## Antes de empezar: qué se hereda y qué no

Un cambio aquí puede alcanzar a **todos los repositorios de la organización**. El agente debe saber
qué está tocando antes de tocarlo:

- GitHub aplica `SECURITY.md`, `SUPPORT.md`, `CODE_OF_CONDUCT.md`, `CONTRIBUTING.md` y las plantillas
  de issues y pull requests a cualquier repositorio que no defina los suyos.
- `profile/README.md` **no se hereda**: es la portada pública de la organización.
- `README.md` tampoco se hereda: describe este repositorio.
- `LICENSE` no se hereda: cada repositorio necesita el suyo.
- Este `AGENTS.md` **no es un archivo especial para GitHub**: no se hereda. Rige aquí, y otro
  repositorio puede adoptarlo copiándolo o enlazándolo.

Si una instrucción del usuario contradice este documento, el agente debe detenerse, señalar el
conflicto y pedir una decisión explícita. El humano siempre es el responsable del cambio.

## 1. Principios no negociables

1. **Plan primero.** No se edita ningún archivo ni se ejecuta ningún comando sin haber anunciado
   antes el objetivo y el plan.
2. **El plan es una propuesta.** El usuario puede cambiarlo, recortarlo o rechazarlo. El agente no
   defiende el plan: lo ajusta.
3. **Ejecución visible.** Durante el trabajo, el agente dice qué está haciendo, qué archivo está
   tocando y qué comando está lanzando.
4. **Cambios pequeños y revisables.** Un cambio = un propósito. El límite es el de `CONTRIBUTING.md`:
   **≤ 5 archivos y ≤ 300 líneas por PR**. Si el plan lo supera, se divide en slices y el humano
   revisa uno antes de que el agente empiece el siguiente. El agente no puede ampliar ese límite por
   su cuenta.
5. **Cierre con resumen.** Al terminar, el agente resume cambios, comandos, decisiones, riesgos y
   próximos pasos.
6. **El humano decide.** Las decisiones de producto, arquitectura, seguridad, dinero, datos,
   credenciales y despliegues requieren aprobación humana.
7. **Sin secretos ni PII real.** Nunca leer, escribir, commitear ni registrar credenciales, tokens,
   claves, datos personales reales o dinero real.
8. **Sin atribución de IA.** No agregar trailers de IA a los commits. El autor humano es el
   accountable.
9. **Radio de impacto.** Antes de editar un archivo heredado, el agente explica a qué repositorios
   afecta y qué se rompe si el archivo cambia de forma.

## 2. Flujo obligatorio

El agente trabaja en cinco fases. No puede saltarse la fase de plan.

### Fase 1 — Contexto

Antes de proponer nada, el agente debe:

- leer `README.md`, `CONTRIBUTING.md` y este `AGENTS.md`;
- revisar el estado del repo (`git status`, rama actual) y detectar archivos sin trackear;
- determinar si el archivo a tocar es heredado y a qué repositorios alcanza;
- consultar la documentación de producto en el repositorio
  [`docs`](https://github.com/vela-bank/docs) cuando el cambio lo requiera;
- identificar supuestos, dependencias y decisiones pendientes.

La documentación de producto no vive aquí: vive en el repositorio `docs`. Este repositorio no tiene
carpeta `docs/` y no debe crearla.

Si falta contexto crítico, lo dice antes de inventar.

### Fase 2 — Plan

El agente presenta un plan con esta estructura:

```markdown
## Plan propuesto

**Objetivo:** ...
**Alcance:** ...
**Fuera de alcance:** ...
**Supuestos:** ...
**Decisiones que necesito:** ...
**Archivos que voy a tocar:** ...
**Impacto de herencia:** ...
**Comandos que voy a ejecutar:** ...
**Riesgos:** ...
**Criterios de aceptación:** ...
**Pasos:**
1. ...
2. ...
3. ...
```

Reglas del plan:

- Debe ser concreto: nombres de archivos, comandos, resultados esperados.
- Debe indicar qué partes son propuestas y qué partes ya están decididas.
- Debe decir si el archivo es heredado y a qué repositorios afecta.
- Si hay una decisión abierta, debe ofrecer opciones con recomendación.
- Si el usuario pidió solo un análisis, el agente **no ejecuta**: termina en el plan.
- Si el usuario pidió ejecutar, el agente puede avanzar después de presentar el plan, pero debe
  detenerse en los puntos de aprobación de la sección 5.
- Todo plan debe declarar los slices y verificar que ninguno supere el límite de tamaño de
  `CONTRIBUTING.md` (≤ 5 archivos, ≤ 300 líneas). Si un slice lo supera, se divide antes de ejecutar.
- Si el usuario quiere revisar, el agente entrega **un slice por vez** y espera aprobación antes de
  empezar el siguiente.

### Fase 3 — Ejecución

Durante la ejecución, el agente anuncia cada paso antes de hacerlo, informa qué archivo crea, edita
o elimina y qué comando exacto lanza, reporta el resultado con errores y códigos de salida, mantiene
el plan actualizado y, antes de commitear o abrir un PR, mide el diff con `git diff --shortstat`. Si
el diff supera el límite de tamaño, se detiene y propone la división. Si el plan cambia, se detiene y
pide aprobación.

```text
Paso 2/6 — Actualizar la portada de la organización.
Voy a editar: profile/README.md
Motivo: reflejar un repositorio nuevo en la tabla de proyectos.

Hecho: profile/README.md
Resultado: 4 líneas añadidas.

Voy a ejecutar: git status --short --branch
Motivo: confirmar que no hay cambios previos antes de editar.
Resultado: rama docs/portada-de-la-organizacion, sin cambios pendientes.
```

### Fase 4 — Verificación

Antes de cerrar, el agente debe verificar lo que hizo:

- revisar el diff o el archivo creado;
- comprobar que toda referencia entre archivos del repositorio resuelve a un archivo que existe;
- comprobar que los enlaces externos apuntan a rutas válidas del repositorio `docs`;
- revisar el render de GitHub cuando el cambio toca tablas, listas o bloques de código;
- comprobar que no se tocaron archivos fuera del plan;
- comprobar que no hay secretos ni PII.

Las validaciones disponibles hoy son la revisión del diff, `markdownlint` y la comprobación de
enlaces cuando las herramientas estén instaladas. No hay `make verify`, ni tests, ni código
ejecutable. Si una validación falla, el agente lo informa y propone el siguiente paso. No oculta
fallos.

### Fase 5 — Resumen

El cierre debe incluir:

```markdown
## Resumen

**Qué se hizo:** ...
**Archivos creados:** ...
**Archivos modificados:** ...
**Comandos ejecutados y resultado:** ...
**Decisiones tomadas:** ... (con enlace al ADR del repositorio docs)
**Decisiones pendientes:** ... (con enlace a docs/decisions-pending.md del repositorio docs)
**Riesgos o limitaciones:** ...
**Verificación:** ...
**Próximo paso propuesto:** ...
```

El resumen debe ser honesto: si algo quedó incompleto, se dice.

## 3. Plantilla de progreso

```text
Paso <n>/<total> — <descripción>
Voy a: <acción>
Archivo/comando: <ruta o comando>
Motivo: <por qué>
```

Las plantillas de plan y de resumen son las de las fases 2 y 5; no se duplican aquí.

## 4. Disciplina de archivos y comandos

### Archivos

- Antes de editar: indicar ruta exacta y motivo.
- Después de editar: indicar qué cambió y cuántas líneas aproximadas.
- Si el archivo es heredado, decir en el plan a qué repositorios afecta.
- No editar archivos fuera del plan sin anunciarlo.
- No borrar archivos sin aprobación explícita.
- No sobrescribir trabajo del usuario sin haberlo leído.
- No reformatear archivos completos si el cambio es puntual.
- No cambiar la redacción de `SECURITY.md`, `SUPPORT.md`, `CODE_OF_CONDUCT.md` ni `CONTRIBUTING.md`
  más allá de lo que exige el cambio: son contratos con toda la organización.

### Comandos

- Antes de ejecutar: mostrar el comando exacto y el motivo.
- Después de ejecutar: mostrar resultado, errores y código de salida.
- No ejecutar comandos destructivos sin aprobación explícita: `rm -rf`, `git reset --hard`,
  `git clean -fd`, `git push --force`.
- No ejecutar `git commit`, `git push`, apertura de PR ni despliegues si el usuario no lo pidió
  explícitamente.
- Cuando el usuario pida commits: un commit por propósito, título en Conventional Commits y sin
  trailers de atribución de IA.
- No ejecutar comandos que consuman credenciales o generen costos sin aprobación.

## 5. Cuándo detenerse y pedir aprobación

El agente debe detenerse antes de continuar cuando:

1. Hay una decisión de arquitectura, producto, seguridad o dinero sin resolver.
2. El cambio afecta producción, datos reales o credenciales.
3. El plan original ya no alcanza y hay que ampliar el alcance.
4. Se necesita un comando destructivo o irreversible.
5. Faltan permisos, accesos o información.
6. Hay instrucciones contradictorias entre el usuario, `CONTRIBUTING.md`, este archivo o los ADRs.
7. Un test o validación falla dos veces seguidas por la misma causa.
8. Se detecta un secreto, PII real o una vulnerabilidad.
9. El agente no puede explicar el impacto del cambio.
10. El diff supera el límite de tamaño de `CONTRIBUTING.md` (≤ 5 archivos, ≤ 300 líneas) o mezcla
    más de un propósito.
11. El cambio toca un archivo heredado y el agente no puede explicar su efecto en los repositorios
    que lo heredan.
12. Una referencia del repositorio apunta a un archivo inexistente y corregirla amplía el alcance
    del plan.

En esos casos, el agente no improvisa: describe la situación, las opciones y la recomendación.

## 6. Decisiones y documentación

Este repositorio no guarda ADRs. La documentación durable vive en el repositorio
[`docs`](https://github.com/vela-bank/docs):

- Toda decisión durable se registra en un ADR de
  [`docs/adr/`](https://github.com/vela-bank/docs/tree/main/docs/adr).
- Toda decisión abierta se registra en
  [`docs/decisions-pending.md`](https://github.com/vela-bank/docs/blob/main/docs/decisions-pending.md).
- Si una decisión cambia un ADR existente, se crea un nuevo ADR que supersede al anterior; no se
  reescribe la historia.
- Si cambia la gobernanza o el mapa de repositorios, se actualizan
  [`docs/governance/`](https://github.com/vela-bank/docs/tree/main/docs/governance) y
  [`docs/architecture/repo-map.md`](https://github.com/vela-bank/docs/blob/main/docs/architecture/repo-map.md).

Lo que sí vive aquí: el texto de los archivos heredados, la portada de la organización en
`profile/README.md` y la descripción de este repositorio en `README.md`.

Si el agente toma una decisión menor durante la ejecución, la informa en el progreso y la deja en el
resumen. Si la decisión es durable, propone el ADR en el repositorio `docs`; no lo escribe por su
cuenta.

## 7. Reglas específicas de este repositorio

- Este repositorio es el `.github` de la organización: contiene gobierno, no producto.
- No hay código ejecutable, ni tests, ni `Makefile`. No se crean servicios ni infraestructura aquí.
- No se crea una carpeta `docs/` local.
- Archivos heredados por todos los repositorios: `SECURITY.md`, `SUPPORT.md`, `CODE_OF_CONDUCT.md`,
  `CONTRIBUTING.md` y las plantillas de issues y pull requests.
- Archivos que **no** se heredan: `README.md`, `profile/README.md`, `AGENTS.md` y `LICENSE`.
- `profile/README.md` es la portada pública: un error ahí se ve en la página de la organización.
- Las referencias entre archivos se mantienen vivas: si `CONTRIBUTING.md` cita un archivo, ese archivo
  existe.
- Este archivo complementa a `CONTRIBUTING.md`; no lo reemplaza. Si se contradicen, el agente se
  detiene y pide la decisión.
- Los documentos marcados como `[PROPUESTO]` no son decisiones cerradas.

## 8. Seguridad

- No leer ni escribir `.env`, secretos, tokens, claves privadas o credenciales.
- No pegar secretos en documentos, issues, PRs, commits ni logs.
- No usar datos personales reales. Solo datos sintéticos.
- No ejecutar código de origen desconocido.
- No agregar dependencias sin justificar y revisar licencia/seguridad.
- No desactivar controles de seguridad para “hacer que funcione”.
- Reportar vulnerabilidades de forma privada, no en un issue público.
- Recordar que estos archivos se publican tal cual en GitHub: un secreto filtrado aquí es público.

## 9. Definition of Done para un cambio asistido por IA

Un cambio está terminado cuando:

- [ ] El plan fue anunciado y el alcance respetado.
- [ ] Los archivos tocados coinciden con el plan, o el cambio fue anunciado.
- [ ] No quedan referencias a archivos inexistentes en este repositorio.
- [ ] El efecto de herencia está explicado, o se declaró que el archivo no se hereda.
- [ ] Los enlaces externos y las tablas se revisaron.
- [ ] Las decisiones durables están propuestas como ADR en el repositorio `docs`.
- [ ] Las validaciones disponibles pasan o el fallo está reportado.
- [ ] No hay secretos ni PII real.
- [ ] El resumen final incluye cambios, comandos, decisiones y próximos pasos.
- [ ] El cambio respeta el límite de tamaño (≤ 5 archivos, ≤ 300 líneas) o tiene una excepción
      aprobada por el humano y registrada en el PR.
- [ ] El humano revisó y aprobó el cambio.

## 10. Relación con otros documentos

- `README.md`: qué es este repositorio y cómo funciona la herencia.
- `CONTRIBUTING.md`: flujo humano de ramas, commits, PRs, releases y seguridad.
- `SECURITY.md`, `SUPPORT.md`, `CODE_OF_CONDUCT.md`: valores por defecto de la organización.
- `profile/README.md`: portada pública de Vela Bank.
- [`docs/vision.md`](https://github.com/vela-bank/docs/blob/main/docs/vision.md): propósito y
  principios del producto.
- [`docs/scope.md`](https://github.com/vela-bank/docs/blob/main/docs/scope.md): alcance del MVP.
- [`docs/architecture/`](https://github.com/vela-bank/docs/tree/main/docs/architecture):
  arquitectura, repositorios, límites y flujos.
- [`docs/adr/`](https://github.com/vela-bank/docs/tree/main/docs/adr): decisiones de arquitectura.
- [`docs/contracts/`](https://github.com/vela-bank/docs/tree/main/docs/contracts): OpenAPI y eventos.
- [`docs/operations/`](https://github.com/vela-bank/docs/tree/main/docs/operations): desarrollo
  local, CI/CD, seguridad, observabilidad y testing.
- [`docs/governance/`](https://github.com/vela-bank/docs/tree/main/docs/governance): organización,
  repositorios y trabajo multi-repo.
