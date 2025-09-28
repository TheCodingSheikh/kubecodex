# Essentials Directory

The structure is similar to `apps/`, but **without a cluster name**:

```
essentials/<PROJECT>/<APP_NAME>/config.yaml
```

## Key Differences

- For each `config.yaml` in `essentials/`, an application will be created **for every cluster registered in Argo CD automatically**.
- The naming convention is the same as in `apps/`, with the cluster name prepended to the generated application name.

## Use Cases

Essentials are perfect for:
- Monitoring and observability tools
- Security scanners
- Log aggregation systems
- Cluster-wide utilities
- Any application that should be deployed to all clusters
