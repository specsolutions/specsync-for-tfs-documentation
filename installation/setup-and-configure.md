# Setup and Configure

Once you have installed SpecSync for Azure DevOps tool, you need to configure it. This means you have to create a SpecSync configuration file \(specsync.json\) that contains the Azure DevOps project connection details and other synchronization settings.

### Step 1: Define feature file sets

SpecSync has to be configured for every _feature file set_. **Feature file set is a folder that contains feature files that you want to synchronize as one batch.**

{% hint style="info" %}
_Feature file set_ is a term introduced by SpecSync to denote the set of feature files that belong together in a platform independent way. Earlier we used the term _project folder_, but it was misleading, because people might call different things as "project" on .NET, Java and node.js platforms and also it could be confused with Azure DevOps projects.

If you use SpecSync with SpecFlow, the Visual Studio projects configured with SpecFlow are the feature file sets.

For other BDD tools, like Cucumber, usually the `features` folder\(s\) are set as feature file sets.
{% endhint %}

Each feature file set will have its own SpecSync configuration file \(`specsync.json`\). For projects with multiple feature file sets, you can use [hierarchical configuration files](../features/general-features/hierarchical-configuration-files.md) as well.

It is up to you how you define your feature file sets. The following table contains a few usual setup variations.

| Platform & tool | Feature file set \(location of specsync.json\) |
| :--- | :--- |
| .NET solution with a single SpecFlow project | The SpecFlow project folder \(e.g. `MyProject.Specs`\) |
| .NET solution with multiple SpecFlow projects | Each SpecFlow project folder \(e.g. `MyProject.BackOffice.Specs` and `MyProject.ClientApp.Specs`\) |
| node.js app with Cucumber.js | The parent folder of the `features` folder \(e.g. `/src/test/`\) |
| node.js app with multiple feature file sets | The parent folders of the `features` folders \(e.g. `/src/test/backoffice/` and `/src/test/clientapp/`\) |
| Java app with Cucumber Java | The parent folder of the `features` folder \(e.g. `/src/test/resources/`\) |

You can also consider setting the features folder itself as a root of the feature file set \(e.g.  `/src/test/resources/features/` instead of `/src/test/resources/`\).

{% hint style="info" %}
The feature file sets of your project and be changed later by moving the `specysnc.json` configuration file to a different location.
{% endhint %}

### Step 2: Initialize configuration file using the SpecSync init command

The easiest way to initialize the configuration file and configure the most important settings is to use the SpecSync `init` command. To use the command open a command window or bash shell and change the current directory to the root folder of the feature file set and invoke the following command.

{% tabs %}
{% tab title=".NET Core tool" %}
```text
dotnet specsync init
```
{% endtab %}

{% tab title=".NET Console App" %}
```text
SpecSync4AzureDevOps.cmd init
```
{% endtab %}

{% tab title="Native binaries" %}
```text
<EXTRACTED-SPECSYNC-FOLDER>/SpecSync4AzureDevOps init
```
{% endtab %}
{% endtabs %}

The init command will ask a few questions in order the setup the connection to the Azure DevOps project, like the project URL and the authentication details. If you are unsure about what is exactly your Azure DevOps project URL, please check the [What is my Azure DevOps project URL](../important-concepts/what-is-my-server-url.md) page. You can fine more information about the authentication options in the [Azure DevOps authentication options](../features/general-features/server-authentication-options.md) page.

{% hint style="success" %}
It is recommended to let the init command test the authentication to the Azure DevOps project to make sure the connection is configured properly.
{% endhint %}

### Step 3: Review SpecSync features and extend configuration settings

The init command configures the settings that are enough for the first synchronization. SpecSync comes with useful default values for all settings, but you can still review the [SpecSync features](../features/) and extend the [configuration ](../reference/configuration/) if you need to. It is recommended to think about whether you would like to [exclude any scenarios from the synchronization](../features/common-synchronization-features/excluding-scenarios-from-synchronization.md) or consider [including all Azure DevOps Test Cases synchronized by SpecSync into a Test Suite](../features/common-synchronization-features/group-synchronized-test-cases-to-a-test-suite.md).

{% page-ref page="../features/" %}

{% page-ref page="../reference/configuration/" %}

### Step 4: Verify setup by synchronizing scenarios

When the SpecSync configuration file is complete, you can perform the first synchronization task and verify the results using the `push` command.

For larger feature file sets it is recommended to make the first synchronization only for a few selected scenarios tagged with a particular tag. You can even temporarily tag a few scenarios in order to test the synchronization. In our example we assume that the selected scenarios are tagged with `@specsync_test`.

To use the command open a command window or bash shell and change the current directory to the root folder of the feature file set and invoke the following command. For all command line option of the push command, please check the [command line reference](../reference/command-line-reference/push-command.md).

{% tabs %}
{% tab title=".NET Core tool" %}
```text
dotnet specsync push --tagFilter @specsync_test
```
{% endtab %}

{% tab title=".NET Console App" %}
```text
SpecSync4AzureDevOps.cmd push --tagFilter @specsync_test
```
{% endtab %}

{% tab title="Native binaries" %}
```text
<EXTRACTED-SPECSYNC-FOLDER>/SpecSync4AzureDevOps push --tagFilter @specsync_test
```
{% endtab %}
{% endtabs %}

The synchronization should run successfully and SpecSync should create Test Cases in Azure DevOps for the selected scenarios and link to them using a tag.

In case of errors, you can retry the synchronization with an additional --verbose option that displays additional diagnostic information. If this does not help, please check the [Troubleshooting](../contact/troubleshooting.md) page or contact [SpecSync Support](../contact/specsync-support.md).

## Setup SpecFlow plugin to support "Test Suite based execution with Scenario Outline wrappers" automation strategy for .NET SpecFlow projects <a id="setup-specflow-plugin"></a>

SpecSync can synchronize scenarios to Azure DevOps Test Cases and publish test execution results without any additional NuGet package.

There is a special way to publish test results that require you to install a SpecFlow plugin as a NuGet package. This special way, the  "Test Suite based execution with Scenario Outline wrappers" automation strategy is described in detail in the [documentation ](../important-concepts/synchronizing-automated-test-cases.md#test-suite-based-execution-with-scenario-outline-wrappers-strategy)that describes the conditions when this strategy has to be used.

The exact NuGet package to be installed depends on the SpecFlow version of your project. For example for SpecFlow `v2.4.*`, install [`SpecSync.AzureDevOps.SpecFlow.2-4`](https://www.nuget.org/packages/SpecSync.AzureDevOps.SpecFlow.2-4). The following table contains the supported SpecFlow versions and the related plugin packages.

| SpecFlow version | SpecFlow plugin NuGet package |
| :--- | :--- |
| v2.3 | [`SpecSync.AzureDevOps.SpecFlow.2-3`](https://www.nuget.org/packages/SpecSync.AzureDevOps.SpecFlow.2-3) |
| v2.4 | [`SpecSync.AzureDevOps.SpecFlow.2-4`](https://www.nuget.org/packages/SpecSync.AzureDevOps.SpecFlow.2-4) |
| v3.0 | [`SpecSync.AzureDevOps.SpecFlow.3-0`](https://www.nuget.org/packages/SpecSync.AzureDevOps.SpecFlow.3-0) |
| v3.1 | [`SpecSync.AzureDevOps.SpecFlow.3-1`](https://www.nuget.org/packages/SpecSync.AzureDevOps.SpecFlow.3-1) |
| v3.3 | [`SpecSync.AzureDevOps.SpecFlow.3-3`](https://www.nuget.org/packages/SpecSync.AzureDevOps.SpecFlow.3-3) |

{% hint style="info" %}
If your project is not configured to use the  "Test Suite based execution with Scenario Outline wrappers" automation strategy, the plugin will not have any impact \(can be safely removed\).
{% endhint %}
