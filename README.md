# Workflows reutilizables

*Dejate de copiar deploy.yml a cada proyecto.*

[Documentación oficial](https://docs.github.com/en/actions/using-workflows/reusing-workflows)


## Workflows disponibles

### deploy-ds.yml

Clona el proyecto al servidor y copia lo necesario a la carpeta destino.

> Importante: Borra archivos no trackeados que no estén excluidos explícitamente

- `inputs`:
  - `deploy_dir`: carpeta destino en `ds-enorme`, tiene que ser una **ruta absoluta** (sino no corre).
  - `exclusiones`: los elementos que no se quieran borrar en el destino o que no se quieran copiar al destino (raro).
    - relativos a la raíz del proyecto.
    - separados por coma y sin espacios por favor.
    - con trailing comma aunque sea una sola exclusión!

**Ejemplo de uso**
```yaml
# scanntech/otro-repo/.github/workflows/deploy.yml
name: deploy

on:
  push:
    branches:
      - main
      - master
jobs:
  deploy:
    uses: scanntech/workflows/.github/workflows/deploy-ds.yml@master
    with:
      deploy_dir: /u01/home/ds/deploy/prod/ejemplo 
      exclusiones: 'env,secretos.txt,'
```

### conda-env-ds.yml

Crea un prefijo de conda a partir de un environment.yml en la carpeta objetivo.

> Si el prefijo ya existe entonces se actualiza

- `inputs`:
  - `env_dir`: carpeta destino en `ds-enorme`, tiene que ser una **ruta absoluta** (sino no corre).
  - `env_yml`: Archivo de definición del entorno, ruta relativa al repositorio

**Ejemplo de uso**
```yaml
# scanntech/otro-repo/.github/workflows/conda.yml
name: build conda

on:
  push:
    branches:
      - master
jobs:
  build:
    uses: scanntech/workflows/.github/workflows/conda-env-ds.yml@master
    with:
      env_dir: /u01/home/ds/deploy/prod/ejemplo/env 
      env_yml: environment.yml
```

### lint-ds.yml

Chequeo statico de archivos de workflows ft. actionlint.

**Ejemplo de uso**
```yaml
# scanntech/otro-repo/.github/workflows/lint.yml
name: Lint workflows

on:
  pull_request:
    paths:
      - '.github/workflows/*.yml'
jobs:
  actionlint:
    uses: scanntech/workflows/.github/workflows/lint-workflows-ds.yml@master
```

# Uso general

- Desde dónde se puede llamar?
    - Desde cualquier lado si es público *(este lo es)*.
    - Desde cualquier otro worklow del mismo repositorio si es privado.
- Con `uses`:
```yaml
jobs:
  llamar-workflow-con-inputs:
    uses: scanntech/workflows/.github/workflows/ejemplo.yml@master # se puede usar otra @rama o @tag
    with:
      username: kevin
    secrets:
      token: shhh
```

## Creación de workflows reutilizables

Ver [CREACION.md](CREACION.md)
