# `.github`

Repositorio de gobierno de la organización [Vela Bank](https://github.com/vela-bank).

## Qué contiene

| Ruta | Función | ¿Se hereda? |
| --- | --- | --- |
| `profile/README.md` | Portada de la organización, la que se ve en el perfil. | No |
| `CONTRIBUTING.md` | Cómo se contribuye: ramas, commits, pull requests y releases. | Sí |
| `AGENTS.md` | Protocolo de trabajo para agentes de IA. | No |
| `PULL_REQUEST_TEMPLATE.md` | Plantilla que precarga el cuerpo de cada pull request. | Sí |
| `.github/ISSUE_TEMPLATE/` | Plantillas de issue y enlaces de contacto. | Sí |
| `SECURITY.md` | Cómo reportar una vulnerabilidad. | Sí |
| `SUPPORT.md` | Qué soporte hay (ninguno) y dónde consultar. | Sí |
| `CODE_OF_CONDUCT.md` | Código de conducta. | Sí |
| `LICENSE` | Apache-2.0 de este repositorio. | No |

## Cómo funciona la herencia

GitHub busca estos archivos en el repositorio donde se abre la página y, si no los encuentra, en
este. Un repositorio que define su propio `SECURITY.md` prevalece sobre este.

Tres excepciones:

- `profile/README.md` no se hereda; solo se muestra en el perfil de la organización.
- `LICENSE` tampoco se hereda: cada repositorio necesita el suyo.
- `AGENTS.md` no es un archivo especial para GitHub: no se hereda, y otro repositorio puede
  adoptarlo copiándolo o enlazándolo.

Los templates de issue se toman de `.github/ISSUE_TEMPLATE/`. Si un repositorio define los suyos,
se usan los suyos y estos se ignoran por completo.

## Alcance

Vela Bank es un proyecto educativo, con dinero simulado y datos sintéticos. No es una entidad
financiera y no debe usarse con datos reales.

## Modificaciones

Los cambios se proponen por pull request, con el mismo flujo que el resto de la organización.
