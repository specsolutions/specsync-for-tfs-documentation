# Command line reference

{% hint style="warning" %}
The documentation is currently being updated. Please check it again soon...
{% endhint %}

SpecSync functions are provided through the command line tool. To use the command line tool open a command window or bash shell and run SpecSync followed by a command and appropriate options. The options and further configuration have to be specified in a [SpecSync configuration file](../configuration/) \(by default `specsync.json`\). The SpecSync command line tool is normally invoked from the folder where the configuration file of the project is saved.

Running SpecSync might require you to use different executable depending on the installation. 

* `dotnet specsync <COMMAND> [options]` — when SpecSync is installed as a platform independent .NET Core tool \(recommended\)
* `SpecSync4AzureDevOps.exe <COMMAND> [options]` — when you use the .NET Framework interface through the SpecSync.AzureDevOps.Console NuGet package on Windows
* `SpecSync4AzureDevOps <COMMAND> [options]` — when you use the native binaries for Linux or MacOS

The available commands and the options are the same for all usage, please check the [Installation & Usage](../../installation.md) page for further information about usage options. In the command line reference we generally use the .NET Core tool usage syntax. 

### Examples

Invoking the following command displays SpecSync version.

```text
dotnet specsync version
```

{% hint style="info" %}
_SpecSync collects anonymous error diagnostics and statistics. Neither user nor machine names, nor Azure DevOps URLs, nor test case & test suite names nor IDs are collected! This can be disabled with the_ `--disableStats` _parameter._
{% endhint %}

## Commands

| Command | SpecSync version | Description |
| :--- | :--- | :--- |
| [init](init-command.md) | 3.0 + | Initializes SpecSync configuration |
| [push](push-command.md) | All | Pushes changes of the scenarios on the local repository to the Azure DevOps server. This includes linking of new scenarios to new test cases \(link\) and updating test cases of linked scenarios \(update\). |
| [pull](pull-command.md) | All | Pulls changes from Azure DevOps server to the local repository. This by default includes creation of new scenarios from unlinked test cases \(create\) and changing scenarios of linked test cases \(change\). |
| [publish-test-results](publish-test-results-command.md) | 2.1 + | Publishes local test results to Azure DevOps server. |
| help | All | Displays help information or help for a command. |
| version | All | Displays SpecSync version. |

## Common command line options

The following command line options are available for all commands that require established configuration \(`push`, `pull`, `publish-test-results`\).

| Option | Description | Default |
| :--- | :--- | :--- |
| &lt;CONFIG‑FILE‑PATH&gt; | The path of the SpecSync configuration file, absolute path or relative to the current folder, e.g. `MyProject.Specs\specsync.json`. Has to be specified as a last parameter. | use `specsync.json` from the current folder |
| `--user` &lt;USER‑NAME&gt; | The Azure DevOps user name or [personal access token](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=vsts) \(PAT\). Overrides `remote/user` setting of the configuration file. See [Azure DevOps authentication options](../../important-concepts/tfs-authentication-options.md) for details. | use from config file or interactive prompt |
| `--password` &lt;PASSWORD&gt; | The password for the Azure DevOps user. Overrides `remote/password` setting of the configuration file. See [Azure DevOps authentication options](../../important-concepts/tfs-authentication-options.md) for details. | use from config file or interactive prompt |
| `--license` &lt;LICENSE‑FILE‑PATH&gt; | The path to the license file; can be relative to the project folder. Overrides `toolSettings/licensePath` setting of the configuration file. See [Licensing](../../licensing.md) for details. \(Default: use from config file or `specsync.lic`\) | use from config file or `specsync.lic` |
| `--baseFolder` &lt;FOLDER&gt; | The base folder where SpecSync searches for project, feature and license files by default. | folder of the configuration file |
| `--disableStats` | If specified, SpecSync will not collect anonymous error diagnostics and statistics. Overrides `toolSettings/disableStats` setting of the configuration file. | false |
| `-v`, `--verbose` | If specified, error messages and trace information will be displayed more in detail. Overrides `toolSettings/outputLevel` setting of the configuration file. | false |
| `--diag` | If specified, diagnostic information will be added to the output. | false |
| `--log` &lt;LOG-FILE&gt; | If specified, the output will also be saved to a log file. | no log file is written |

## Examples

Display SpecSync version:

```text
dotnet specsync version
```

Get help about command line options for `push` command:

```text
dotnet specsync help push
```

Synchronize local changes to Azure DevOps and save execution log to a file `log.txt`:

```
dotnet specsync push --log log.txt
```

