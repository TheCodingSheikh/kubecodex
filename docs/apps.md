# Applications Directory

Applications are defined in the following structure:

```
apps/<CLUSTER>/<PROJECT>/<APP_NAME>/config.yaml
```

## Directory Structure

- `<CLUSTER>`: The name of the cluster where the app will be deployed. This cluster **must be registered in Argo CD**.
- `<PROJECT>`: The Argo CD project name. The project **must be created** (see the `projects/` directory for how to define projects).
- `<APP_NAME>`: The application name, which can be any directory structure. The actual Argo CD application name is generated automatically.

## Nested Directories

You can also use nested directories for the app, e.g.:

```
apps/<CLUSTER>/<PROJECT>/**/config.yaml
```

This allows for more complex application organization while maintaining the same configuration structure.
