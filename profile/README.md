# Vela Bank

Banco minorista **simulado**, construido de punta a punta para practicar ingeniería de software
bancaria: contabilidad de doble entrada, idempotencia, eventos confiables, entrega continua y
gobierno multi-repositorio.

> **No es un banco real.** El dinero es simulado y los datos, sintéticos. El proyecto no está
> afiliado a ninguna entidad financiera, no acepta depósitos ni retiros, y no debe usarse para
> operar con dinero real.

## Qué demuestra

- **Ledger de doble entrada** con asientos inmutables y balance verificado por invariantes.
- **Idempotencia** en transferencias: clave única más hash del request, sin doble débito.
- **Outbox transaccional** para publicar eventos sin romper la consistencia.
- **Gobierno multi-repositorio**: equipos, `CODEOWNERS`, rulesets y plantillas compartidas.
- **GitOps**: la imagen se construye una vez y se promueve por digest entre entornos.
- **Infraestructura como código**: la organización, los repositorios y los recursos de AWS se
  declaran en Terraform; nada se configura a mano.
- **Decisiones registradas**: cada elección relevante vive en un ADR, con contexto y alternativas.
- **AWS emulado** con [Floci](https://github.com/floci-io/floci) en local, sin depender de una
  cuenta real durante el desarrollo.

## Repositorios

| Repositorio | Propósito |
| --- | --- |
| `docs` | Fuente de verdad: visión, alcance, arquitectura, ADRs y dominio. |
| `contracts` | Contratos OpenAPI y eventos, con control de cambios incompatibles. |
| `platform` | Entorno local: Docker Compose, Floci, `kind` y Argo CD. |
| `service-template` | Estándar de servicio: Spring Boot, Flyway, Testcontainers y Helm. |
| `core-banking-service` | Dominio contable: clientes, cuentas, ledger y extractos. |
| `transfers-service` | Transferencias con idempotencia, orquestación y outbox. |
| `gitops` | Estado declarativo de los entornos. |
| `web` | Aplicación de cliente. |

Los repositorios se crean cuando tienen contenido, owner y pipeline. Este perfil se actualiza con
sus enlaces a medida que existen.

## Stack

Java 21 · Spring Boot · PostgreSQL 16 · SQS/SNS · Kubernetes (`kind` en local, EKS como objetivo) ·
Argo CD · Terraform para infraestructura como código · GitHub Actions con SBOM y firma de imágenes.

## Estado

Fase de documentación: la arquitectura y las decisiones están cerradas, y el código llega con el
*walking skeleton*.

## Licencia

Apache-2.0.
