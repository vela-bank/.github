# Política de seguridad

## Alcance

Vela Bank es un proyecto educativo: dinero simulado, datos sintéticos y sin usuarios reales. No debe
desplegarse para operar ni usarse con información personal o financiera real.

Aun así, el código se escribe como si fuera a producción, y los fallos de seguridad se tratan en
privado.

## Cómo reportar una vulnerabilidad

**No abras un issue público.**

Usa **Security → Report a vulnerability** en el repositorio afectado. Ese canal es privado y llega
solo al mantenedor. Si el problema cruza varios repositorios, repórtalo en cualquiera de ellos e
indica el alcance.

Incluye, si puedes:

- repositorio, commit o versión afectada;
- impacto esperado y qué permite hacer;
- pasos para reproducirlo, con datos sintéticos;
- propuesta de mitigación.

## Qué esperar

| Etapa | Plazo orientativo |
| --- | --- |
| Acuse de recibo | 7 días |
| Evaluación inicial | 14 días |
| Corrección o decisión | según severidad, sin SLA |

Es un proyecto mantenido por una persona: no hay guardia, ni recompensas económicas, ni compromiso
de plazos.

## Qué no se considera vulnerabilidad

- Falta de endurecimiento en un entorno que se declara simulado.
- Avisos de dependencias sin impacto demostrable en este proyecto; para eso está Dependabot.
- Ausencia de funcionalidades o de controles propios de un banco regulado.
- Configuración de despliegues locales hechos por terceros.

## Reglas para quien investiga

- No accedas a datos que no sean tuyos y no uses datos reales.
- No pruebes contra infraestructura que no hayas desplegado tú.
- No publiques detalles antes de que exista una corrección o una decisión de no corregir.
