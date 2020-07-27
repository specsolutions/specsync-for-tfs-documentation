# Usage

The SpecSync [install package](../installation.md) contains a command line tool \(`SpecSync4AzureDevOps.exe`\) inside the `tools` folder. All synchronization operations can be performed by invoking this tool from the local environment or [from the CI build process](../important-concepts/synchronizing-test-cases-from-build.md). \(For .NET projects, the package adds a `specsync4azuredevops.cmd` script file to the project for calling the SpecSync command line tool conveniently.\)

The synchronization tool provides different commands. For synchronizing the scenarios to Azure DevOps, the `push` command can be used. The configuration options have to be provided in a [json configuration file](../configuration/), called `specsync.json` by default. It is recommended to invoke the command line tool from the project folder, otherwise the path of the configuration file has to be specified explicitly.

```text
path-to-specsync-package\tools\SpecSync4AzureDevOps.exe push
```

For a detailed setup instructions, please check the [Getting started](../getting-started/) guide. For a complete list of configuration options check the [Configuration](../configuration/) documentation.

_Note: SpecSync collects anonymous error diagnostics and statistics. Neither user nor machine names, nor Azure DevOps URLs, nor test case & test suite names nor IDs are collected! This can be disabled with the_ `--disableStats` _parameter._

## Available SpecSync commands

* `push` -- Pushes changes of the scenarios on the local repository to the Azure DevOps server. This by default includes linking of new scenarios to new test cases \(link\) and updating test cases of linked scenarios \(update\).
* `pull` -- Pulls changes from Azure DevOps server to the local repository. This by default includes creation of new scenarios from unlinked test cases \(create\) and changing scenarios of linked test cases \(change\). See [Two-way synchronization](../important-concepts/two-way-synchronization.md) for details.
* `publish-test-results` -- Publish local test results to Azure DevOps server. See more details about the command in the "Assembly based execution strategy" section of the  [Synchronizing automated test cases](../important-concepts/synchronizing-automated-test-cases.md) article.
* `help` -- Displays more information on a specific command.
* `version` -- Displays version information.

## Common command line options

The following command line options are available for all synchronization commands.

* `[config-file-path]` -- The path of the SpecSync configuration file, absolute path or relative to the current folder, e.g. `MyProject.Specs\specsync.json`. \(Default: use `specsync.json` from the current folder\) 
* `--user [user-name]` -- The Azure DevOps user name or [personal access token](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=vsts) \(PAT\). Overrides `remote/user` setting of the configuration file. See [Azure DevOps authentication options](../important-concepts/tfs-authentication-options.md) for details. \(Default: \[use from config file or interactive prompt\]\)
* `--password` -- The password for the Azure DevOps user. Overrides `remote/password` setting of the configuration file. See [Azure DevOps authentication options](../important-concepts/tfs-authentication-options.md) for details. \(Default: \[use from config file or interactive prompt\]\)
* `--buildServerMode` -- If specified, only those changes will be performed that do not need any change in the local feature file. Linking new scenarios or pulling changes from Azure DevOps are skipped. Overrides `synchronization/enableLocalChanges` setting of the configuration file. See [Synchronizing test cases from build](../important-concepts/synchronizing-test-cases-from-build.md) for details. \(Default: false\) 
* `--tagFilter` -- A [tag expression](http://speclink.me/tagexpressions) of scenarios that should be included in the current synchronization \(e.g. `@current_sprint and @done`\). See [Filters and scopes](../important-concepts/filters-and-scopes.md) for details. \(Default: \[not filtered by tags\]\) 
* `--force` -- If specified, SpecSync update test cases even if there is no local change and the test case was not modified remotely. \(Default: false\)
* `--license` -- The path to the license file; can be relative to the project folder. Overrides `toolSettings/licensePath` setting of the configuration file. See [Licensing](../licensing.md) for details. \(Default: \[use from config file or `specsync.lic`\]\)
* `--baseFolder` -- The base folder where SpecSync searches for project, feature and license files by default. \(Default: \[folder of the configuration file\]\)
* `--disableStats` -- If specified, SpecSync will not collect anonymous error diagnostics and statistics. Overrides `toolSettings/disableStats` setting of the configuration file. \(Default: false\)
* `-v`, `--verbose` -- If specified, error messages and trace information will be displayed more in detail. Overrides `toolSettings/outputLevel` setting of the configuration file. \(Default: false\)

## Synchronization command line options \(for push and pull\)

All common command line options can be used and in addition to that the following options can be specified for `push` and `pull` commands.

* `--buildServerMode` -- If specified, only those changes will be performed that do not need any change in the local feature file. Linking new scenarios or pulling changes from Azure DevOps are skipped. Overrides `synchronization/enableLocalChanges` setting of the configuration file. See [Synchronizing test cases from build](../important-concepts/synchronizing-test-cases-from-build.md) for details. \(Default: false\) 
* `--tagFilter` -- A [tag expression](http://speclink.me/tagexpressions) of scenarios that should be included in the current synchronization \(e.g. `@current_sprint and @done`\). See [Filters and scopes](../important-concepts/filters-and-scopes.md) for details. \(Default: \[not filtered by tags\]\) 
* `--force` -- If specified, SpecSync update test cases even if there is no local change and the test case was not modified remotely. \(Default: false\)

## Publish test results command line options \(for publish-test-results\)

All common command line options can be used and in addition to that the following options can be specified for `publish-test-results` command.

See more details about the command in the "Assembly based execution strategy" section of the [Synchronizing automated test cases](../important-concepts/synchronizing-automated-test-cases.md) article.

* `--testConfiguration` -- The Azure DevOps test configuration name or ID to publish the results for. For specifying an ID, use `#1234` format. \(Default: \[use from config file\]\)
* `--testResultFile` -- The file path of the TRX test result file to publish. \(Default: \[use from config file\]\)

## Examples

Synchronize local changes to Azure DevOps \(`specsync.json` config file is in the current folder\):

```text
path-to-specsync-package\tools\SpecSync4AzureDevOps.exe push
```

Get help about command line options for `push` command:

```text
path-to-specsync-package\tools\SpecSync4AzureDevOps.exe help push
```

Synchronize local changes to Azure DevOps with custom config file:

```text
path-to-specsync-package\tools\SpecSync4AzureDevOps.exe push MyProject.Specs\specsync-group-a.json
```

Synchronize local changes to Azure DevOps on build server:

```text
path-to-specsync-package\tools\SpecSync4AzureDevOps.exe push --buildServerMode
```

