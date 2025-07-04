# Kubecodex

a GitOps repository structure standard using ArgoCD.

## Structure Overview

- `apps/` — Cluster and project-specific applications, organized as `apps/<CLUSTER>/<PROJECT>/<APP_NAME>/`.
- `essentials/` — Essential applications, deployed to all clusters , organized as `essentials/<PROJECT>/<APP_NAME>/`.
- `projects/` — Argo CD AppProject definitions for logical grouping and access control.
- `bootstrap/` — Bootstrap manifests for Argo CD and ApplicationSet resources.
- `cluster-resources/<CLUSTER>/` — Cluster-wide resources

## Applications Directory (`apps/`)

Applications are defined in the following structure:

```
apps/<CLUSTER>/<PROJECT>/<APP_NAME>/config.yaml
```

- `<CLUSTER>`: The name of the cluster where the app will be deployed. This cluster **must be registered in Argo CD**.
- `<PROJECT>`: The Argo CD project name. The project **must be created** (see the `projects/` directory for how to define projects).
- `<APP_NAME>`: The application name, which can be any directory structure. The actual Argo CD application name is generated automatically.

You can also use nested directories for the app, e.g.:

```
apps/<CLUSTER>/<PROJECT>/**/config.yaml
```

## Essentials Directory (`essentials/`)

The structure is similar to `apps/`, but **without a cluster name**:

```
essentials/<PROJECT>/<APP_NAME>/config.yaml
```

- For each `config.yaml` in `essentials/`, an application will be created **for every cluster registered in Argo CD automatically**.
- The naming convention is the same as in `apps/`, with the cluster name prepended to the generated application name.

## Cluster Resources Directory (`cluster-resources/`)

This directory contains cluster-wide resources for each cluster.

```
cluster-resources/<CLUSTER>/*.yaml
```

- For each cluster, an application will be created **for every cluster registered in Argo CD automatically**.
- Any YAML files placed in `cluster-resources/<CLUSTER>/*.yaml` will be applied to the specified cluster.

## Usage of `config.yaml`

- The presence of a `config.yaml` file in either `apps/` or `essentials/` **triggers the creation of an Argo CD Application** via ApplicationSet.
- The `config.yaml` file can be **empty**. All fields have default values calculated from the directory structure and ApplicationSet templates.
- You can override any of the following fields in `config.yaml`:
  - `appName`: The name of the Argo CD Application 
  - `destNamespace`: The destination namespace 
  - `destServer`: The destination cluster/server 
  - `repoURL`: The Git repository URL 
  - `srcPath`: The path in the repository 
  - `labels`: Additional labels for the Application 
  - `annotations`: Additional annotations for the Application 

### Default Values

| Field           | Default Value                                                                                 |
|-----------------|---------------------------------------------------------------------------------------------|
| `appName`       | `<CLUSTER>-<nested-directory-structure>` 
| `destNamespace` | `<nested-directory-structure>` (directory structure under project, with slashes as dashes)   |
| `destServer`    | The cluster name from the directory structure (for apps/), or each registered cluster (for essentials/) |
| `repoURL`       | The repository URL where the config.yaml resides                                            |
| `srcPath`       | The path to the directory containing the config.yaml                                        |
| `labels`        | None                                                                                        |
| `annotations`   | None                                                                                        |

You can override any of these values by specifying them in your `config.yaml`.


## Example Directory Structures

### `apps/` Example

```
apps/
  in-cluster/
    system/
        config.yaml --> 
      kyverno/
        config.yaml -->
    monitoring/
      prometheus/
        config.yaml -->
```

### `essentials/` Example

```
essentials/
  cert-manager/
    config.yaml
  external-dns/
    config.yaml
```

## Adding Applications

1. Add a new directory and `config.yaml` under `apps/<CLUSTER>/<PROJECT>/<APP_NAME>/` or `essentials/<APP_NAME>/`.
2. Fill out the `config.yaml` with the application details (or leave it empty to use defaults).
3. The ApplicationSet will automatically detect and manage the new application.

## Example `config.yaml`

```yaml
# This file can be empty, or you can override any of the following fields:
# appName, destNamespace, destServer, repoURL, srcPath, labels, annotations
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-app # optional override
  labels:
    my-label: value
  annotations:
    my-annotation: value
spec:
  source:
    repoURL: https://github.com/example/repo.git # optional override
    path: path/to/app # optional override
    targetRevision: HEAD
  destination:
    namespace: my-namespace # optional override
    server: https://kubernetes.default.svc # optional override
```

## More Information

- See `bootstrap/essentials.yaml`, `bootstrap/cluster-resources.yaml`, and `projects/system.yaml` for ApplicationSet examples.
- For project definitions, see `projects/`.

## Bootstrap Directory (`bootstrap/`)

This directory contains the initial manifests for bootstrapping Argo CD and the ApplicationSet controllers, as well as the main ApplicationSet definitions that automate the discovery and management of applications and resources in the repository.

## Projects Directory (`projects/`)

This directory contains Argo CD AppProject definitions. Projects are used to logically group applications, set access controls, and define deployment destinations. Each project must be defined here before it can be referenced in the apps/ or essentials/ directories. 