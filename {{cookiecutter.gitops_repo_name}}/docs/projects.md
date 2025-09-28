# Projects Directory

The `projects/` directory contains Argo CD AppProject and ApplicationSet definitions for logical grouping and access control.

## Default: Helm Chart Approach

Projects are managed using the Kubecodex Helm chart by default:

- **`kustomization.yaml`** - References the kubecodex Helm chart
- **`values.yaml`** - Contains project configuration and repository settings
- Projects are defined in the `values.yaml` file under the `projects` array

### Helm Chart Information
- **Repository:** [https://thecodingsheikh.github.io/helm-charts](https://thecodingsheikh.github.io/helm-charts)
- **Chart Name:** kubecodex
- **Version:** 1.0.0
- **Configuration:** Define projects in the `projects` array in `values.yaml`

### Adding Projects (Helm)
```yaml
projects:
  - project-1
  - project-2
  - my-new-project
```

## Alternative: CLI Approach

If you prefer direct YAML management, remove the Helm chart files and use the CLI:

1. Remove `kustomization.yaml` and `values.yaml`
2. Create projects: `./kubecodex project <name>`
3. This generates direct YAML manifests (`project-1.yaml`)

## Features
- Logical grouping of applications
- Access control and permissions
- Automatic application discovery
- Sync policy management
