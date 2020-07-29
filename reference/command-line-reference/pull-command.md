# pull

Pulls changes from Azure DevOps server to the local repository. This by default includes creation of new scenarios from unlinked test cases \(create\) and changing scenarios of linked test cases \(change\).

## Options

In addition the the options listed here, all [common command line options](./#common-command-line-options) can also be used.

| Option | Description | Default |
| :--- | :--- | :--- |
| `--tagFilter` | A [tag expression](http://speclink.me/tagexpressions) of scenarios that should be included in the current synchronization \(e.g. `@current_sprint and @done`\). See [Filters and scopes](../../important-concepts/filters-and-scopes.md) for details. | not filtered by tags |
| `--force` | If specified, SpecSync changes scenarios even if there is no remote change and the scenario was not modified locally. | false |

## Examples

Pulls remote changes from Azure DevOps using the configured settings in the `specsync.json` configuration file:

```text
dotnet specsync pull
```

Pulls remote changes using the specified [Personal Access Token \(PAT\)](../../features/general-features/tfs-authentication-options.md#vsts-personal-access-tokens) for authentication:

```text
dotnet specsync pull --user 52yny...........................nycsetda
```

Pulls remote changes related to the scenarios tagged with @ordering and @backend:

```text
dotnet specsync pull --tagFilter "@ordering and @backend"
```

{% page-ref page="./" %}

