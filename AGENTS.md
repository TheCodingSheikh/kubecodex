# AGENTS.md

This repository is a [Kubecodex](https://github.com/TheCodingSheikh/kubecodex)-style GitOps repo driven by ArgoCD ApplicationSets.

## Top-level layout

| Path | Purpose |
|---|---|
| `apps/<CLUSTER>/<PROJECT>/<APP>/` | Cluster + project specific apps (ApplicationSet `apps`) |
| `essentials/<PROJECT>/<APP>/` | Apps deployed to **every** registered cluster (ApplicationSet `essentials`) |
| `projects/` | ArgoCD `AppProject`s, defined via the `kubecodex` Helm chart (`values.yaml`) |
| `bootstrap/` | Root ArgoCD `Application` + the discovery `ApplicationSet`s |
| `cluster-resources/<CLUSTER>/*.yaml` | Cluster-wide raw manifests applied per cluster |
| `docs/` | Authoritative docs for the structure and `config.yaml` overrides |
| `kubecodex` | CLI: `./kubecodex setup|bootstrap|project <name>` |

## Discovery rule

The presence of a `config.yaml` file under `apps/**` or `essentials/**` is what causes an ArgoCD `Application` to be created (the ApplicationSets in `bootstrap/` glob for `*/config.yaml`). The file may be empty — every field has a default derived from the directory path. Override-able keys: `appName`, `destNamespace`, `destServer`, `repoURL`, `srcPath`, `srcTargetRevision`, `autoSync`, `createNamespace`, `additionalSyncOptions`, `labels`, `annotations`, `ignoreDifferences`. See `docs/config-yaml.md`.

To **disable** an app without deleting it, rename `config.yaml` → `config.yaml.disabled` (the glob won't match). Example: `apps/in-cluster/datastore/postgres/pgadmin/config.yaml.disabled`.

## App-adding conventions (the kustomize way)

The repo uses Kustomize's `helmCharts:` inflator (not ArgoCD's native Helm support). The downloaded chart tarballs land in a sibling `charts/` directory, which is **gitignored** (see `.gitignore`).

There are three shapes for an app. Pick the one that matches the situation:

### 1. Single-chart app

```
<app>/
├── config.yaml              # empty or with overrides — triggers the Argo Application
├── kustomization.yaml       # resources: [chart]
├── chart/
│   ├── kustomization.yaml   # helmCharts: [...]
│   └── values.yaml
└── charts/                  # gitignored cache of downloaded chart tarballs
```

Example: `apps/in-cluster/monitoring/grafana/`, `essentials/network/ingress-nginx/`.

`<app>/chart/kustomization.yaml` looks like:

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
helmCharts:
  - name: grafana
    repo: https://grafana.github.io/helm-charts
    version: 8.10.0
    releaseName: grafana
    namespace: monitoring
    valuesFile: values.yaml
    includeCRDs: true
```

### 2. Multi-chart, single Argo Application

When one logical app needs multiple Helm releases or extra raw manifests, keep a single `config.yaml` at the root and aggregate everything in the root `kustomization.yaml`. Each Helm release lives in its own subdir with a nested `chart/` directory.

```
<app>/
├── config.yaml
├── kustomization.yaml       # resources: [chart, <subapp1>, <subapp2>, extra.yaml]
├── chart/                   # main release
│   ├── kustomization.yaml   # helmCharts: [...]
│   └── values.yaml
├── <subapp1>/
│   ├── kustomization.yaml   # resources: [chart]
│   └── chart/               # secondary release
│       ├── kustomization.yaml
│       └── values.yaml
├── <subapp2>/
│   └── ...
└── extra.yaml               # raw manifests
```

Example: `apps/in-cluster/security/vault/` has `chart/` + `vault-unseal/` + `iac/` + `pushsecret.yaml`, all referenced from the root `kustomization.yaml`. `apps/in-cluster/datastore/mariadb/` and `.../postgres/` follow the same shape.

### 3. Umbrella with independent sub-apps (multiple Argo Applications)

When the subapps should each be their own ArgoCD Application, the parent has **no** `config.yaml`. Each child gets its own `config.yaml` and its own `chart/`.

```
<umbrella>/
├── <subapp1>/
│   ├── config.yaml          # subapp1 = its own Argo Application
│   ├── kustomization.yaml
│   └── chart/
└── <subapp2>/
    ├── config.yaml
    ├── kustomization.yaml
    └── chart/
```

Examples: `apps/in-cluster/platform/vcluster/{cozystack,kubeflow,remote}/`, `apps/in-cluster/ai/ollama/qwen3-0.6b/`, `apps/in-cluster/monitoring/opentelemetry/collector/{agent,gateway}/`.
