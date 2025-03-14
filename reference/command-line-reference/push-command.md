# push

Pushes changes of the scenarios on the local repository to the Azure DevOps server. This includes linking of new scenarios to new test cases \(link\) and updating test cases of linked scenarios \(update\).

See more details about the command in the [Pushing scenario changes to Test Cases](../../features/push-features/pushing-scenario-changes-to-test-cases.md) article.

## Options

In addition the the options listed here, all [common command line options](./#common-command-line-options) can also be used.

| Option | Description | Default |
| :--- | :--- | :--- |
| `--disableLocalChanges` | If specified, only those changes will be performed that do not need any change in the local feature file. Linking new scenarios are skipped. Overrides `synchronization/diableLocalChanges` setting of the configuration file. See [Synchronizing test cases from build](../../important-concepts/synchronizing-test-cases-from-build.md) for details. | local changes enabled |
| `--tagFilter` | A [local test case condition](../../features/general-features/local-test-case-conditions.md) of scenarios that should be included in the current synchronization \(e.g. `@current_sprint and @done`\). See [Filters and scopes](../../important-concepts/filters-and-scopes.md) for details. | not filtered by tags |
| `--sourceFileFilter` | An expression of source file [glob patterns](https://speclink.me/specsync-glob) that should be included in the current synchronization (e.g. `Folder1/**/*.feature`). See [Filters and scopes](../../important-concepts/filters-and-scopes.md) for details. | not filtered by feature files |
| `--linkOnly` | If specified, the command will link new scenarios to new Test Cases only and the existing Test Cases will not be updated. | false |
| `--force` | If specified, SpecSync update test cases even if there is no local change and the test case was not modified remotely. | false |

## Examples

Pushes local changes to Azure DevOps using the configured settings in the `specsync.json` configuration file:

```text
dotnet specsync push
```

Pushes local changes using the specified [Personal Access Token \(PAT\)](../../features/general-features/server-authentication-options.md#vsts-personal-access-tokens) for authentication:

```text
dotnet specsync push --user 52yny...........................nycsetda
```

Pushes local changes of the scenarios tagged with @ordering and @backend:

```text
dotnet specsync push --tagFilter "@ordering and @backend"
```

Pushes local changes of the scenarios in `order.feature`):

```text
dotnet specsync push --sourceFileFilter "order.feature"
```

{% page-ref page="./" %}

