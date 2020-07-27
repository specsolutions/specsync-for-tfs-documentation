# Command line reference

{% hint style="warning" %}
The documentation is currently being updated. Please check it again soon...
{% endhint %}

SpecSync functions are provided through the command line tool. To use the command line tool open a command window or bash shell and run SpecSync followed by a command and appropriate options. The options and further configuration have to be specified in a [SpecSync configuration file](configuration/) \(by default `specsync.json`\). The SpecSync command line tool is normally invoked from the folder where the configuration file of the project is saved.

Running SpecSync might require you to use different executable depending on the installation. 

* `dotnet specsync [specsync-command] [specsync-options]` — when SpecSync is installed as a platform independent .NET Core tool \(recommended\)
* `SpecSync4AzureDevOps.exe [specsync-command] [specsync-options]` — when you use the .NET Framework interface through the SpecSync.AzureDevOps.Console NuGet package on Windows
* `SpecSync4AzureDevOps [specsync-command] [specsync-options]` — when you use the native binaries for Linux or MacOS

The available commands and the options are the same for all usage, please check the [Installation & Usage](../installation.md) page for further information about usage options. In the command line reference we generally use the .NET Core tool usage syntax. 

{% hint style="info" %}
_SpecSync collects anonymous error diagnostics and statistics. Neither user nor machine names, nor Azure DevOps URLs, nor test case & test suite names nor IDs are collected! This can be disabled with the_ `--disableStats` _parameter._
{% endhint %}

## Commands

| Command | SpecSync version | Description |
| :--- | :--- | :--- |
| init | 3.0 + | Initializes SpecSync configuration |
| push | All | Pushes changes of the scenarios on the local repository to the Azure DevOps server. This includes linking of new scenarios to new test cases \(link\) and updating test cases of linked scenarios \(update\). |
| pull | All | Pulls changes from Azure DevOps server to the local repository. This by default includes creation of new scenarios from unlinked test cases \(create\) and changing scenarios of linked test cases \(change\). |
| publish-test-results | 2.1 + | Publishes local test results to Azure DevOps server. |
| help | All | Displays help information or help for a command. |
| version | All | Displays SpecSync version. |

## Common command line options

The following command line options are available for all synchronization commands.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Option</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>[config-file-path]</code>
      </td>
      <td style="text-align:left">
        <p></p>
        <p>The path of the SpecSync configuration file, absolute path or relative
          to the current folder, e.g. <code>MyProject.Specs\specsync.json</code>.</p>
      </td>
      <td style="text-align:left">use <code>specsync.json</code> from the current folder</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p></p>
        <p>--user [user-name]</p>
      </td>
      <td style="text-align:left">
        <p></p>
        <p>The Azure DevOps user name or <a href="https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=vsts">personal access token</a> (PAT).
          Overrides <code>remote/user</code> setting of the configuration file. See
          <a
          href="../important-concepts/tfs-authentication-options.md">Azure DevOps authentication options</a>for details.</p>
      </td>
      <td style="text-align:left">
        <p></p>
        <p>use from config file or interactive prompt</p>
      </td>
    </tr>
  </tbody>
</table>

* `[config-file-path]` — The path of the SpecSync configuration file, absolute path or relative to the current folder, e.g. `MyProject.Specs\specsync.json`. \(Default: use `specsync.json` from the current folder\) 
* `--user [user-name]` — The Azure DevOps user name or [personal access token](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=vsts) \(PAT\). Overrides `remote/user` setting of the configuration file. See [Azure DevOps authentication options](../important-concepts/tfs-authentication-options.md) for details. \(Default: use from config file or interactive prompt\)
* `--password [password]` — The password for the Azure DevOps user. Overrides `remote/password` setting of the configuration file. See [Azure DevOps authentication options](../important-concepts/tfs-authentication-options.md) for details. \(Default: use from config file or interactive prompt\)
* `--license [license-file-path]` — The path to the license file; can be relative to the project folder. Overrides `toolSettings/licensePath` setting of the configuration file. See [Licensing](../licensing.md) for details. \(Default: use from config file or `specsync.lic`\)
* `--baseFolder` — The base folder where SpecSync searches for project, feature and license files by default. \(Default: \[folder of the configuration file\]\)
* `--disableStats` — If specified, SpecSync will not collect anonymous error diagnostics and statistics. Overrides `toolSettings/disableStats` setting of the configuration file. \(Default: false\)
* `-v`, `--verbose` — If specified, error messages and trace information will be displayed more in detail. Overrides `toolSettings/outputLevel` setting of the configuration file. \(Default: false\)
* `--diag` — If specified, diagnostic information will be added to the output. \(Default: false\)
* `--log [log-file]` — If specified, the output will also be saved to a log file. \(Default: no log file is written\)

{% hint style="warning" %}
The documentation is currently being updated. Please check it again soon...
{% endhint %}

The SpecSync [install package](../installation.md) contains a command line tool \(`SpecSync4AzureDevOps.exe`\) inside the `tools` folder. All synchronization operations can be performed by invoking this tool from the local environment or [from the CI build process](../important-concepts/synchronizing-test-cases-from-build.md). \(For .NET projects, the package adds a `specsync4azuredevops.cmd` script file to the project for calling the SpecSync command line tool conveniently.\)

The synchronization tool provides different commands. For synchronizing the scenarios to Azure DevOps, the `push` command can be used. The configuration options have to be provided in a [json configuration file](configuration/), called `specsync.json` by default. It is recommended to invoke the command line tool from the project folder, otherwise the path of the configuration file has to be specified explicitly.

```text
path-to-specsync-package\tools\SpecSync4AzureDevOps.exe push
```

For a detailed setup instructions, please check the [Getting started](../getting-started/) guide. For a complete list of configuration options check the [Configuration](configuration/) documentation.

_Note: SpecSync collects anonymous error diagnostics and statistics. Neither user nor machine names, nor Azure DevOps URLs, nor test case & test suite names nor IDs are collected! This can be disabled with the_ `--disableStats` _parameter._

## Available SpecSync commands

* `push` -- Pushes changes of the scenarios on the local repository to the Azure DevOps server. This by default includes linking of new scenarios to new test cases \(link\) and updating test cases of linked scenarios \(update\).
* `pull` -- Pulls changes from Azure DevOps server to the local repository. This by default includes creation of new scenarios from unlinked test cases \(create\) and changing scenarios of linked test cases \(change\). See [Two-way synchronization](../important-concepts/two-way-synchronization.md) for details.
* `publish-test-results` -- Publish local test results to Azure DevOps server. See more details about the command in the "Assembly based execution strategy" section of the  [Synchronizing automated test cases](../important-concepts/synchronizing-automated-test-cases.md) article.
* `help` -- Displays more information on a specific command.
* `version` -- Displays version information.

## 

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

