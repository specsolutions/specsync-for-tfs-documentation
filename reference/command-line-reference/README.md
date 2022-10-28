# Command line reference

SpecSync functions are provided through the command line tool. To use the command line tool open a command window or bash shell and run SpecSync followed by a command and appropriate options. The options and further configuration have to be specified in a [SpecSync configuration file](../configuration/) \(by default `specsync.json`\). The SpecSync command line tool is normally invoked from the folder where the configuration file of the project is saved.

Running SpecSync might require you to use different executable depending on the installation. 

* `dotnet specsync <COMMAND> [options]` — when SpecSync is installed as a platform independent .NET tool \(recommended\)
* `SpecSync4AzureDevOps.exe <COMMAND> [options]` — when you use the .NET Framework interface through the SpecSync.AzureDevOps.Console NuGet package on Windows
* `SpecSync4AzureDevOps <COMMAND> [options]` — when you use the native binaries for Linux or MacOS

The available commands and the options are the same for all usage, please check the [Installation & Usage](../../installation/) page for further information about usage options. In the command line reference we generally use the .NET tool usage syntax. 

### Exit Codes

The command line tool terminated with a specific exit code depending on the execution result.

| Exit Code | Description |
| :--- | :--- |
| 0 | Completed |
| 5 | Warnings \(use `--zeroExitCodeForWarnings` to use zero exit code\) |
| 10 | Failed with a synchronization error |
| 90 | Failed with an unhandled error |
| 100 | Failed with a configuration error |

### Examples

Invoking the following command displays SpecSync version.

```text
dotnet specsync version
```

{% hint style="info" %}
_SpecSync collects anonymous error diagnostics and statistics. Neither user nor machine names, nor Azure DevOps URLs, nor test case & other work item names nor IDs are collected! This can be disabled with the_ `--disableStats` _parameter._
{% endhint %}

## Commands

| Command | Description |
| :--- | :--- |
| [init](init-command.md) | Initializes SpecSync configuration |
| [push](push-command.md) | Pushes changes of the scenarios on the local repository to the Azure DevOps server. This includes linking of new scenarios to new test cases \(link\) and updating test cases of linked scenarios \(update\). |
| [pull](pull-command.md) | Pulls changes from Azure DevOps server to the local repository. This by default includes creation of new scenarios from unlinked test cases \(create\) and changing scenarios of linked test cases \(change\). |
| [publish-test-results](publish-test-results-command.md) | Publishes local test results to Azure DevOps server. |
| [re-link](re-link-command.md) | Re-links scenarios (local test cases) to cloned Test Cases in Azure DevOps. |
| help | Displays help information or help for a command. |
| version | Displays SpecSync version and license information. |

## Common command line options

The following command line options are available for all commands that require established configuration \(`push`, `pull`, `publish-test-results`\).

| Option | Description | Default |
| :--- | :--- | :--- |
| &lt;CONFIG‑FILE‑PATH&gt; | The path of the SpecSync configuration file, absolute path or relative to the current folder, e.g. `MyProject.Specs\specsync.json`. Has to be specified as a last parameter. | use `specsync.json` from the current folder |
| `--user` &lt;USER‑NAME&gt; | The Azure DevOps user name or [personal access token](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=vsts) \(PAT\). Overrides `remote/user` setting of the configuration file. See [Azure DevOps authentication options](../../features/general-features/server-authentication-options.md) for details. | use from config file or interactive prompt |
| `--password` &lt;PASSWORD&gt; | The password for the Azure DevOps user. Overrides `remote/password` setting of the configuration file. See [Azure DevOps authentication options](../../features/general-features/server-authentication-options.md) for details. | use from config file or interactive prompt |
| `--ignoreCertificateErrorsForThumbprint` | The thumbprint of the server certificate that should be treated as trusted. It is recommended to install trusted certificates on the operating system instead of using this setting. See related [Troubleshooting entry](../../contact/troubleshooting.md#authentication-ssl-error-the-remote-certificate-is-invalid-according-to-the-validation-procedure-when-connecting-to-an-azure-devops-server-on-promises) for details. | SSL is verified by the OS |
| `--license` &lt;LICENSE‑FILE‑PATH&gt; | The path to the license file; can be relative to the project folder. Overrides `toolSettings/licensePath` setting of the configuration file. See [Licensing](../../licensing.md) for details. \(Default: use from config file or `specsync.lic`\) | use from config file or `specsync.lic` |
| `--disableStats` | If specified, SpecSync will not collect anonymous error diagnostics and statistics. Overrides `toolSettings/disableStats` setting of the configuration file. | false |
| `-v`, `--verbose` | If specified, diagnostic information will be added to the output. Overrides `toolSettings/outputLevel` setting of the configuration file. | false |
| `--log` &lt;LOG-FILE&gt; | If specified, the output will also be saved to a log file. | no log file is written |
| `--zeroExitCodeForWarnings` | If specified, the command line tool will terminate with 0 exit code even in case of warnings. | Non-zero exit code is returned for warnings |
| `--configOverride` | Can be used to override configuration file settings. This option can be used multiple times. See [Override configuration setting from command line](#override-configuration-setting-from-command-line) for details. | No configuration setting overrides |
| `--dryRun` | If specified, the command will be performed, but no actual change is made either to Azure DevOps or to the local test files (feature files). This option is useful for testing the impact of an operation without making an actual change. | Normal mode |


## Override configuration setting from command line

With the `--configOverride` option any configuration file setting can be overridden even if there is no dedicated command line option for that. Using this option can be used as a backup solution to handle special cases. As an alternative to this option, you can also consider using [hierarchical configuration files](../../features/general-features/hierarchical-configuration-files.md#parent-configuration-files).

The following example overrides the user name setting in the `remote` configuration section (equivalent to the exposed `--user` option).

```text
dotnet specsync push --configOverride "remote/user=myusername"
```

{% hint style="warning" %}
Most of the configuration settings changes the synchronization behavior and therefore changing them might cause a modification for many Test Cases. Therefore it is generally not recommended to use the `--configOverride` option. Please contact [support](../../contact/specsync-support.md) if you are unsure about how to solve a particular synchronization requirement with SpecSync.
{% endhint %}

In order to override multiple configuration setting, the `--configOverride` option can be used multiple times or you can specify multiple settings separated by a semicolon \(`;`\).

The value for the `--configOverride` option is a *configuration setter* that has to follow a certain format: `<path>=<value>`, where

* `<path>` is the path of the configuration setting, e.g. `remote/testSuite/name`. For overriding values of list \(array\) settings, you can use the following path values:
  * `<array-path>[<index>]` -- overrides a setting at a specific index. E.g. `synchronization/links[0]/tagPrefix` can be used to set or override the `tagPrefix` setting of the first `link` element.
  * `<array-path>[]` -- adds a new element to the end of the list. E.g. `synchronization/links[]/tagPrefix` can be used to add a new link element and set the `tagPrefix` setting.
  * `<array-path>[-<index>]` -- overrides a setting at a specific index backwards (-1 is the last element, -2 is the one before last, etc.). E.g. `synchronization/links[-1]/tagPrefix` can be used to set or override the `tagPrefix` setting of the last `link` element. This setting can also be used in combination with the `[]` syntax. E.g. the following two setting adds a new link elements and sets two settings of it: `synchronization/links[]/tagPrefix=bug;synchronization/links[-1]/relationship=tests`
* `<value>` is the new value of the setting. The option recognizes numbers, booleans \(`true`/`false`\) and arbitrary strings.

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

