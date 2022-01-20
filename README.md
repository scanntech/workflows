# Workflows reutilizables

*Dejate de copiar deploy.yml a cada proyecto.*

[Documentación oficial](https://docs.github.com/en/actions/using-workflows/reusing-workflows)


## workflows

### deploy-ds.yml

Clona el proyecto al servidor y copia lo necesario a la carpeta destino.

> Importante: Borra archivos no trackeados que no estén excluidos explícitamente

Archivos necesarios (e.g. credenciales) que no se agregan al repositorio tienen 

- `inputs`:
  - `deploy_dir`: carpeta destino en `ds-enorme`, tiene que ser una **ruta absoluta** (sino no corre).
  - `exclusiones`: los elementos que no se quieran borrar en el destino o que no se quieran copiar al destino (raro).
    - relativos a la raíz del proyecto.
    - separados por coma y sin espacios por favor.

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
      exclusiones: 'env,secretos.txt'
```

## Creación de workflows

- Poner el `ejemplo.yml` en la carpeta `.github/workflows` de este repositorio.
- Tiene que incluir `on/workflow_call` (puede aceptar `inputs` y/o `secrets`):
```yaml
on:
  workflow_call:
    inputs:
          username:
            required: true
            type: string
        secrets:
          token:
            required: true
```
## Uso

- Desde dónde se puede llamar?
    - Desde cualquier lado si es público *(este lo es)*.
    - Desde cualquier otro worklow del mismo repositorio si es privado.
- Con `uses`:
```yaml
jobs:
  llamar-workflow-con-inputs:
    uses: scanntech/workflows/.github/workflows/ejemplo.yml@master # @master es implícito, se puede usar otra @rama o @tag
    with:
      username: kevin
    secrets:
      token: shhh
```
