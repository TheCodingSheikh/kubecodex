# Structure Overview

The Kubecodex repository follows a standardized structure for managing GitOps deployments with ArgoCD:

- `apps/` — Cluster and project-specific applications, organized as `apps/<CLUSTER>/<PROJECT>/<APP_NAME>/`.
- `essentials/` — Essential applications, deployed to all clusters, organized as `essentials/<PROJECT>/<APP_NAME>/`.
- `projects/` — Argo CD AppProject definitions for logical grouping and access control.
- `bootstrap/` — Bootstrap manifests for Argo CD and ApplicationSet resources.
- `cluster-resources/<CLUSTER>/` — Cluster-wide resources

This structure provides clear separation of concerns and enables automated application discovery and deployment through ArgoCD ApplicationSets.
