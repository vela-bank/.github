# `.github`

Repositorio de gobierno de la organización [Vela Bank](https://github.com/vela-bank).

## Qué contiene

| Ruta | Función |
| --- | --- |
| `profile/README.md` | Portada de la organización. No se hereda: es la que se ve en el perfil. |
| `SECURITY.md` | Cómo reportar una vulnerabilidad. Valor por defecto para todos los repos. |
| `SUPPORT.md` | Qué soporte hay (ninguno) y dónde consultar. Valor por defecto. |
| `CODE_OF_CONDUCT.md` | Código de conducta. Valor por defecto. |

## Cómo funciona la herencia

GitHub busca estos archivos en el repositorio donde se abre la página y, si no los encuentra, en
este. Un repositorio que define su propio `SECURITY.md` prevalece sobre este.

Dos excepciones:

- `profile/README.md` no se hereda; solo se muestra en el perfil de la organización.
- `LICENSE` tampoco se hereda: cada repositorio necesita el suyo.

## Alcance

Vela Bank es un proyecto educativo, con dinero simulado y datos sintéticos. No es una entidad
financiera y no debe usarse con datos reales.

## Modificaciones

Los cambios se proponen por pull request, con el mismo flujo que el resto de la organización.
