# push

Pushes changes of the scenarios on the local repository to the Azure DevOps server. This includes linking of new scenarios to new test cases \(link\) and updating test cases of linked scenarios \(update\).

## Options

In addition the the options listed here, all [common command line options](./#common-command-line-options) can also be used.

| Option | Description | Default |
| :--- | :--- | :--- |
| `--disableLocalChanges` | If specified, only those changes will be performed that do not need any change in the local feature file. Linking new scenarios are skipped. Overrides `synchronization/diableLocalChanges` setting of the configuration file. See [Synchronizing test cases from build](../../important-concepts/synchronizing-test-cases-from-build.md) for details. | local changes enabled |
| `--tagFilter` | A [tag expression](http://speclink.me/tagexpressions) of scenarios that should be included in the current synchronization \(e.g. `@current_sprint and @done`\). See [Filters and scopes](../../important-concepts/filters-and-scopes.md) for details. | not filtered by tags |
| `--force` | If specified, SpecSync update test cases even if there is no local change and the test case was not modified remotely. | false |

## Examples

Pushes local changed to Azure DevOps using the configured settings in the `specsync.json` configuration file:

```text
dotnet specsync push
```

Pushes local changes using the specified [Personal Access Token \(PAT\)](../../important-concepts/tfs-authentication-options.md#vsts-personal-access-tokens) for authentication:

```text
dotnet specsync push --user 52yny...........................nycsetda
```

Pushes local changes of the scenarios tagged with @ordering and @backend:

```text
dotnet specsync push --tagFilter "@ordering and @backend"
```

