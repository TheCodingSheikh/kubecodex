# Essentials Directory

The structure is similar to `apps/`, but **without a cluster name**:

```
essentials/<PROJECT>/<APP_NAME>/config.yaml
```

## Key Differences

- For each `config.yaml` in `essentials/`, an application will be created **for every cluster registered in Argo CD automatically**,
  unless that `config.yaml` narrows the set with `include` or `exclude` — see [Cluster targeting](#cluster-targeting) below.
- The naming convention is the same as in `apps/`, with the cluster name prepended to the generated application name.

## Use Cases

Essentials are perfect for:
- Monitoring and observability tools
- Security scanners
- Log aggregation systems
- Cluster-wide utilities
- Any application that should be deployed to all clusters

## Cluster targeting

Not every essential belongs on every cluster. A remote cluster may have no
route to the dependencies an app expects, and a small cluster may not want the
whole stack.

Two keys in an app's `config.yaml` narrow the default of "every registered
cluster". They are **mutually exclusive**:

```yaml
exclude:
  - cluster-1
```

Deploys to every registered cluster except `cluster-1`.

```yaml
include:
  - cluster-1
  - cluster-2
```

Deploys only to `cluster-1` and `cluster-2`, and nowhere else.
