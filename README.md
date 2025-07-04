# Kubecodex

a GitOps repository structure standard using ArgoCD.

## Quick Start

Generate your GitOps repo structure directly from this template without cloning:

```bash
cookiecutter https://github.com/TheCodingSheikh/kubecodex.git
```

When prompted, fill in your desired variables (e.g., `gitops_repo_name`, etc.).

Then navigate to your generated directory and push it to a git repo, then apply the bootstrap application to install initial ArgoCD resources:
* ArgoCD must be deployed already.

```bash
./kubecodex bootstrap
```
or
```bash
kubectl apply -f bootstrap.yaml
```

That’s it — your ArgoCD instance will start syncing resources from your GitOps repository!

To make ArgoCD manage itself, uncomment `resources` in bootstrap/argo-cd/kustomization.yaml

Of course — here’s a short, professional section you can add to your README to acknowledge your inspirations:

## Example

This is a full example you can view and get ideas from [kubecodex](https://github.com/TheCodingSheikh/kubecodex/tree/example) 


## Inspiration

This project was inspired by the ideas and best practices from:

* [kubezero](https://github.com/kubezero/kubezero) 
* [ArgoCD Autopilot](https://argocd-autopilot.readthedocs.io) 


## Structure Overview

- `apps/` — Cluster and project-specific applications, organized as `apps/<CLUSTER>/<PROJECT>/<APP_NAME>/`.
- `essentials/` — Essential applications, deployed to all clusters , organized as `essentials/<PROJECT>/<APP_NAME>/`.
- `projects/` — Argo CD AppProject definitions for logical grouping and access control.
- `bootstrap/` — Bootstrap manifests for Argo CD and ApplicationSet resources.
- `cluster-resources/<CLUSTER>/` — Cluster-wide resources

## Bootstrap Directory (`bootstrap/`)

This directory contains the initial manifests for bootstrapping Argo CD and the ApplicationSet controllers, as well as the main ApplicationSet definitions that automate the discovery and management of applications and resources in the repository.

## Projects Directory (`projects/`)

This directory contains Argo CD AppProject and ApplicationSet definitions. Projects are used to logically group applications, set access controls, and define deployment destinations. Each project must be defined here before it can be referenced in the apps/ or essentials/ directories.

Use `./kubecodex project <name>` to create projects easily, or copy a project and change th respective names manually

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
  - `srcTargetRevision`: The branch in the repository
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
| `srcTargetRevision`       | `HEAD`                                       |
| `labels`        | None                                                                                        |
| `annotations`   | None                                                                                        |

You can override any of these values by specifying them in your `config.yaml`.

## Contribution

We would love your ideas and examples