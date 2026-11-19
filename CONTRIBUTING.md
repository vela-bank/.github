# Guía de contribución

> Guía por defecto de todos los repositorios de la organización `vela-bank`. GitHub la aplica a
> cualquier repositorio que no defina su propio `CONTRIBUTING.md`.

Todos los repositorios siguen un flujo **trunk-based de estilo bancario**: cambios pequeños, ramas
de vida corta, una `main` protegida y una ruta controlada de promoción de staging a producción.

## 1. Principios

- **Pequeño y reversible.** Un solo propósito por cambio. Mejor muchos PRs pequeños que uno grande.
- **Aprobación humana.** Una persona revisa y aprueba cada cambio antes de que llegue a `main`.
  Ver la [sección 12](#12-desarrollador-único) para la excepción vigente.
- **`main` siempre en verde.** Cada commit compila, pasa los tests y se puede desplegar.
- **Los tests son la especificación.** Un cambio de comportamiento incluye sus tests.
- **Sin secretos ni PII real.** Se usa `.env.example` y datos sintéticos.
- **La IA asiste, las personas deciden.** Ver la [sección 11](#11-trabajo-asistido-por-ia).

## 2. Modelo de ramas

```text
main ──●──●──●──●──●──●──●──►   protegida, desplegable a staging
        \        \
         feat/S01  release/0.1 ──► tag v0.1.0 ──► producción
                      \
                       hotfix/0.1.1 ──► tag v0.1.1 ──► producción
```

| Rama | Propósito | Vida |
| --- | --- | --- |
| `main` | Rama de integración; siempre en verde | Permanente |
| `feat/<slice>-<slug>` | Capacidad nueva, p. ej. `feat/S01-accounts-domain` | ≤ 2 días |
| `fix/<slug>` | Corrección de un fallo | ≤ 2 días |
| `docs/<slug>` | Solo documentación | ≤ 2 días |
| `chore/<slug>` | Herramientas, dependencias, refactors sin cambio de comportamiento | ≤ 2 días |
| `ci/<slug>` | Pipelines y automatización | ≤ 2 días |
| `release/<major.minor>` | Estabilización de una versión | Hasta etiquetarla |
| `hotfix/<major.minor.patch>` | Corrección urgente desde una etiqueta | Hasta etiquetarla |

Reglas:

1. `main` está protegida. No se hace push directo.
2. Se parte de una `main` actualizada y se hace rebase antes de abrir el PR.
3. Una rama = un PR = un propósito.
4. Si una rama dura más de dos días, se divide el trabajo.
5. Las ramas se conservan después del merge para mantener la trazabilidad; no se borran.

## 3. Commits

Se usa [Conventional Commits](https://www.conventionalcommits.org/es/v1.0.0/):

```text
<tipo>(<alcance>): <descripción>
```

| Tipo | Uso |
| --- | --- |
| `feat` | Comportamiento nuevo |
| `fix` | Corrección de un fallo |
| `docs` | Solo documentación |
| `test` | Solo tests |
| `refactor` | Cambio interno sin cambio de comportamiento |
| `chore` | Herramientas, dependencias y mantenimiento |
| `ci` | Cambios de CI/CD |
| `perf` | Mejora de rendimiento |
| `build` | Cambios del sistema de build |

Ejemplos:

```text
feat(accounts): añadir el agregado de retenciones
fix(transfers): liberar la retención cuando falla el asiento
docs(adr): registrar la decisión de propiedad del ledger
ci: añadir la etapa de tests de integración
```

Reglas:

- Descripción en infinitivo, en minúscula, sin punto final y de ≤ 72 caracteres.
- El cuerpo explica **por qué**, no qué; se parte a 72 caracteres.
- Si existe un issue, se referencia en el pie: `Refs #12`.
- No se agregan trailers de atribución de IA. La persona autora responde por el cambio.
- Los commits firmados son recomendados. Pasarán a ser obligatorios cuando la firma esté
  configurada en la organización.
- Los commits nuevos usan el email `noreply` de GitHub, según el
  [ADR-0013](https://github.com/vela-bank/docs/blob/main/docs/adr/0013-public-visibility.md).

## 4. Pull requests

Todo cambio entra por un pull request, también la documentación.

**Límite de tamaño (estricto):** ≤ 300 líneas cambiadas y ≤ 5 archivos, sin contar lockfiles ni
código generado. Es una regla de revisabilidad: una persona debe poder leer el diff completo de una
sentada.

- Un PR = un propósito. No se mezclan decisiones, refactors, formato y cambios de comportamiento.
- Si el cambio supera el límite, se divide en PRs apilados **antes** de abrirlo.
- Las excepciones requieren la aprobación explícita de quien revisa, registrada en la descripción
  del PR con el motivo y el plan de seguimiento. «Todo está relacionado» no basta.
- En cambios asistidos por IA no hay excepción automática: el agente propone la división y espera
  aprobación antes de continuar.

**Título:** sigue Conventional Commits, porque el squash merge lo usa como commit en `main`.

**Descripción:** GitHub precarga el cuerpo del PR desde
[`PULL_REQUEST_TEMPLATE.md`](PULL_REQUEST_TEMPLATE.md). Un repositorio con su propia plantilla
reemplaza a esta.

**Revisión y merge:**

| Tipo de cambio | Estrategia de merge |
| --- | --- |
| `feat`, `fix`, `docs`, `chore`, `ci`, `refactor` | **Squash merge** |
| Backports de `release` y `hotfix` | Merge commit o cherry-pick, y luego etiqueta |

- Se requiere al menos una aprobación (ver la [sección 12](#12-desarrollador-único)).
- El CI debe estar en verde y las conversaciones resueltas.
- No se hace force-push sobre una rama compartida.
- Las ramas se conservan después del merge; no se borran.

## 5. Flujo de un pull request

El mismo flujo vale para documentación y para código:

```bash
git switch main
git pull --ff-only
git switch -c docs/acuerdo-de-trabajo
# editar archivos
git add CONTRIBUTING.md
git commit -m "docs: añadir la guía de contribución"
git push -u origin docs/acuerdo-de-trabajo
```

Después se completa la integración en la plataforma:

1. Abrir un pull request contra `main` y referenciar el issue relacionado.
2. Esperar los checks requeridos y al menos una aprobación.
3. Hacer **squash merge** desde la plataforma; no integrar en local con `git merge`.
4. Conservar la rama después del merge; no borrarla.

La plataforma registra el PR, la revisión, los checks y el merge como pista de auditoría del
cambio.

## 6. CI/CD y promoción

El pipeline sigue el principio **construir una vez, promover muchas**. El detalle de las etapas, la
promoción por digest y el rollback está en
[`docs/operations/ci-cd.md`](https://github.com/vela-bank/docs/blob/main/docs/operations/ci-cd.md).
Todo PR debe pasar los checks que allí se definen antes del merge.

## 7. Versiones

- Se sigue [Versionado Semántico](https://semver.org/lang/es/).
- Se crea `release/<major.minor>` desde `main` cuando el hito está listo, y solo entran correcciones.
- La versión se etiqueta como `v<major.minor.patch>` y se actualiza `CHANGELOG.md`.
- Una versión no está completa hasta que existen la etiqueta, el changelog y el registro del
  despliegue.

Los cambios coordinados entre repositorios se describen en
[Trabajo multi-repositorio](https://github.com/vela-bank/docs/blob/main/docs/governance/multi-repo-workflow.md).

## 8. Issues y slices

- Un issue por unidad de trabajo (un «slice»), con criterios de aceptación y plan de pruebas.
- El PR se vincula con `Closes #<issue>`.
- Etiquetas sugeridas: `type:*`, `area:*`, `priority:*`, `status:*`.
- Un slice está listo cuando su alcance, sus criterios de aceptación y su verificación están
  escritos.

## 9. Verificación

Cambios de documentación:

- `markdownlint` y comprobación de enlaces, cuando exista la herramienta.
- Revisión humana de exactitud y alcance.

Cambios de código:

- `make verify` en local: formato, lint, unitarios, integración, contratos y arquitectura.
- El CI debe estar en verde antes del merge.
- Todo PR indica cómo se verificó e incluye la salida de los comandos cuando sea relevante.

## 10. Seguridad

- Nunca se versionan secretos. `.env.example` documenta las variables necesarias.
- Las vulnerabilidades se reportan en privado según [`SECURITY.md`](SECURITY.md); nunca en un issue
  público.
- El diseño de seguridad del sistema está en
  [`docs/operations/security.md`](https://github.com/vela-bank/docs/blob/main/docs/operations/security.md).
- Se ejecutan detección de secretos y análisis de dependencias en el CI.
- Solo se usan datos sintéticos. Este proyecto nunca debe contener datos reales de clientes.

## 11. Trabajo asistido por IA

La IA es una herramienta, no la autora. La persona responde por cada cambio.

Todo cambio asistido por IA sigue el protocolo de [`AGENTS.md`](AGENTS.md):

1. El agente anuncia el objetivo y un plan antes de actuar.
2. El plan es una propuesta abierta a cambios y revisión.
3. Durante la ejecución, el agente informa cada archivo que toca y cada comando que ejecuta.
4. El agente verifica el resultado y no oculta los fallos.
5. El agente cierra con un resumen de cambios, decisiones, riesgos y próximos pasos.
6. El agente propone slices dentro del límite de tamaño y espera la revisión antes de empezar el
   siguiente.
7. Si un slice superara el límite, el agente se detiene y pregunta; no abre un PR más grande.

El agente no hace commits, push, pull requests, despliegues ni comandos destructivos sin un pedido
explícito. Las decisiones de producto, arquitectura, seguridad, dinero y credenciales requieren
aprobación humana.

No se permiten trailers de atribución de IA. La persona autora responde por el cambio.

## 12. Desarrollador único

GitHub no permite que una persona apruebe su propio pull request. Mientras el proyecto tenga un
único mantenedor:

- El mantenedor revisa el diff completo antes de hacer el merge.
- Los checks requeridos siguen siendo obligatorios; no se relajan.
- La regla de «al menos una aprobación» se aplica con una cuenta revisora independiente cuando
  exista.

Es una excepción temporal, no una relajación permanente de la regla.
