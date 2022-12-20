# pull

Pulls changes from Azure DevOps server to the local repository. This by default includes creation of new scenarios from unlinked test cases \(create\) and changing scenarios of linked test cases \(change\).

See more details about the command in the [Pulling Test Case changes to local scenarios](../../features/pull-features/two-way-synchronization.md) article.

## Options

In addition the the options listed here, all [common command line options](./#common-command-line-options) can also be used.

| Option | Description | Default |
| :--- | :--- | :--- |
| `--tagFilter` | A [local test case condition](../../features/general-features/local-test-case-conditions.md) of scenarios that should be included in the current synchronization \(e.g. `@current_sprint and @done`\). See [Filters and scopes](../../important-concepts/filters-and-scopes.md) for details. | not filtered by tags |
| `--sourceFileFilter` | A [local test case condition](../../features/general-features/local-test-case-conditions.md) with source file predicates (glob patterns) of files that should be included in the current synchronization (e.g. `Folder1/**/*.feature`). See [Filters and scopes](../../important-concepts/filters-and-scopes.md) for details. | not filtered by feature files |
| `--createOnly` | If specified, the command will create new scenarios for the unlinked Test Cases only and the existing scenarios will not be modified. This setting automatically enables the 'enableCreatingScenariosForNewTestCases' pull setting. | false |
| `--force` | If specified, SpecSync changes scenarios even if there is no remote change and the scenario was not modified locally. | false |


## Examples

Pulls remote changes from Azure DevOps using the configured settings in the `specsync.json` configuration file:

```text
dotnet specsync pull
```

Pulls remote changes using the specified [Personal Access Token \(PAT\)](../../features/general-features/server-authentication-options.md#vsts-personal-access-tokens) for authentication:

```text
dotnet specsync pull --user 52yny...........................nycsetda
```

Pulls remote changes related to the scenarios tagged with @ordering and @backend:

```text
dotnet specsync pull --tagFilter "@ordering and @backend"
```

Pulls unlinked Test Cases to new scenarios but ignores changed Test Cases:

```text
dotnet specsync pull --createOnly
```

{% page-ref page="./" %}

