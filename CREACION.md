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
