# Projects Directory

The `projects/` directory contains Argo CD AppProject and ApplicationSet definitions for logical grouping and access control.

## Default: Helm Chart Approach

Projects are managed using the Kubecodex Helm chart by default:

- **`kustomization.yaml`** - References the kubecodex Helm chart
- **`values.yaml`** - Contains project configuration and repository settings
- Projects are defined in the `values.yaml` file under the `projects` array
- **Repository:** [https://github.com/TheCodingSheikh/helm-charts/tree/main/charts/kubecodex](https://github.com/TheCodingSheikh/helm-charts/tree/main/charts/kubecodex)



### Adding Projects (Helm)
```yaml
projects:
  - name: project-1
    preserveResourcesOnDeletion: false # Default is false if not set
  - name: project-2
    preserveResourcesOnDeletion: true
  - name: new-project
```

## Alternative: CLI Approach

If you prefer direct YAML management, remove the Helm chart files and use the CLI:

1. Remove `kustomization.yaml` and `values.yaml`
2. Create projects: `./kubecodex project <name>`
3. This generates direct YAML manifests (`project-1.yaml`)
