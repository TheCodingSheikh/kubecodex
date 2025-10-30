# Config.yaml Usage

The presence of a `config.yaml` file in either `apps/` or `essentials/` **triggers the creation of an Argo CD Application** via ApplicationSet.

## Configuration File

The `config.yaml` file can be **empty**. All fields have default values calculated from the directory structure and ApplicationSet templates.

## Override Fields

You can override any of the following fields in `config.yaml`:

- `appName`: The name of the Argo CD Application 
- `destNamespace`: The destination namespace 
- `destServer`: The destination cluster/server 
- `repoURL`: The Git repository URL 
- `srcPath`: The path in the repository 
- `srcTargetRevision`: The branch in the repository
- `autoSync`: Whether to disable autoSync (enabled by default)
- `createNamespace`: Whether to create the destination namespace if it does not exist (enabled by default)
- `additionalSyncOptions`: Additional sync options for the Application. This can include Argo CD [sync options](https://argo-cd.readthedocs.io/en/stable/user-guide/sync-options/) such as `Prune`, `ApplyOutOfSyncOnly`, etc.
- `ignoreDifferences`: Array of fields to ignore in the Argo App [diffing customization](https://argo-cd.readthedocs.io/en/stable/user-guide/diffing/)
- `labels`: Additional labels for the Application 
- `annotations`: Additional annotations for the Application 

## Default Values

| Field           | Default Value                                                                                 |
|-----------------|---------------------------------------------------------------------------------------------|
| `appName`       | `<CLUSTER>-<nested-directory-structure>` if app in `essentials/` the default value becomes `essential-<CLUSTER>-<nested-directory-structure>`
| `destNamespace` | `<nested-directory-structure>` (directory structure under project, with slashes as dashes)   |
| `destServer`    | The cluster name from the directory structure (for apps/), or each registered cluster (for essentials/) |
| `repoURL`       | The repository URL where the config.yaml resides                                            |
| `srcPath`       | The path to the directory containing the config.yaml                                        |
| `srcTargetRevision`       | `HEAD`                                       |
| `autoSync`       | `true`                                       |
| `createNamespace` | `true`                                                                                       |
| `additionalSyncOptions` | None                                                                                   |
| `ignoreDifferences` | None                                                                                   |
| `labels`        | None                                                                                        |
| `annotations`   | None                                                                                        |

You can override any of these values by specifying them in your `config.yaml`.
