# Unit Testing Workflow

Este repositorio contiene un workflow de GitHub Actions reusable para ejecutar pruebas unitarias en proyectos Node.js. Está diseñado para ser llamado desde otros repositorios mediante la funcionalidad [workflow_call](https://docs.github.com/en/actions/using-workflows/reusing-workflows).

## Características

Escaneo automático de secretos en el código usando [trufflehog](https://github.com/trufflesecurity/trufflehog)

- Análisis de dependencias con [depcheck](https://github.com/depcheck/depcheck) para detectar dependencias no usadas

- Ejecución de pruebas con cobertura y reporte en formatos texto, HTML y lcov

- Validación automática de la cobertura mínima configurada

- Reporte y comentarios automáticos en Pull Requests sobre vulnerabilidades o cobertura

- Configuración flexible con inputs para rama, runner y cobertura mínima

## Uso

Para reutilizar este workflow desde otro repositorio, añade un job que lo invoque con `uses`. Por ejemplo:

```yaml
name: 🕵 Running Unit Tests

on:
  pull_request:
    paths-ignore:
      - '**/*.md'

jobs:
  coverage:
    uses: hmanzur/test-node-action/.github/workflows/unit-testing.yaml@main
    with:
      ref: ${{ github.head_ref }}
      runner: ubuntu-latest
      minimum_coverage: 35
```

## Inputs

| Nombre             | Tipo   | Obligatorio | Valor por defecto | Descripción                                              |
| ------------------ | ------ | ----------- | ----------------- | -------------------------------------------------------- |
| `ref`              | string | No          | `github.ref`      | Rama o tag que se va a testear.                          |
| `runner`           | string | No          | `ubuntu-latest`   | Imagen del runner donde correr el job.                   |
| `minimum_coverage` | number | No          | `80`              | Porcentaje mínimo de cobertura para pasar la validación. |
| `eslint_config`    | string | No          | `eslint.config.js` | Path al archivo de configuración de ESLint.             |


## Requisitos

Proyecto Node.js con tests configurados y script `test:coverage` para ejecutar las pruebas con cobertura.

Archivo `.node-version` opcional para definir la versión de Node.js (por defecto se usa la versión que se configura en el workflow).

Token con permisos para comentar PRs (por defecto usa secrets.GITHUB_TOKEN).

## Contribuciones

Contribuciones y sugerencias son bienvenidas. Abre un issue o PR para discutir cambios.