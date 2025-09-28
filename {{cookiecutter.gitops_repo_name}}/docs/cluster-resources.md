# Cluster Resources Directory

This directory contains cluster-wide resources for each cluster.

```
cluster-resources/<CLUSTER>/*.yaml
```

## Key Features

- For each cluster, an application will be created **for every cluster registered in Argo CD automatically**.
- Any YAML files placed in `cluster-resources/<CLUSTER>/*.yaml` will be applied to the specified cluster.

## Use Cases

Cluster resources are ideal for:
- Namespace definitions
- RBAC configurations
- Network policies
- Storage classes
- Custom resource definitions (CRDs)
- Cluster-wide configurations
