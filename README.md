# Kubecodex

Kubecodex is a standardized GitOps repository structure that works with ArgoCD to manage Kubernetes applications across multiple clusters. It provides:

- **Organized Structure**: Clear separation between cluster-specific and essential applications
- **Automated Discovery**: ApplicationSet controllers automatically discover and manage applications
- **Flexible Configuration**: Override defaults through simple config.yaml files
- **Multi-Project Support**: Logical grouping and access control through ArgoCD projects

## Setup

After cloning or creating from template, start by using the CLI:

```bash
./kubecodex setup
```

It'll prompt for:
- Repository URL
- ArgoCD namespace

Then you can apply the bootstrap manifests (Argo should already be installed)

```bash
./kubecodex bootstrap
```
or
```bash
kubectl apply -f bootstrap.yaml
```

That's it — your ArgoCD instance will start syncing resources from your GitOps repository!

To make ArgoCD manage itself, uncomment `resources` in bootstrap/argo-cd/kustomization.yaml

## Example

This is a full example you can view and get ideas from [kubecodex](https://github.com/TheCodingSheikh/kubecodex/tree/example) 


## Inspiration

This project was inspired by the ideas and best practices from:

* [kubezero](https://github.com/kubezero/kubezero) 
* [ArgoCD Autopilot](https://argocd-autopilot.readthedocs.io) 

## Getting Started

1. Read the [Structure Overview](docs/structure-overview.md) to understand the directory layout
2. Follow the [Quick Start Guide](docs/index.md#quick-start) to set up your first project
3. Explore the [Configuration Guide](docs/config-yaml.md) to customize your deployments

For detailed information about each component, see the [main documentation index](docs/index.md).
