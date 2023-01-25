# How to use SpecSync from build or release pipeline

Keeping the test cases in sync with the scenarios is important, therefore automating the synchronization process is recommended. This can be done for example from CI/CD build or release pipeline: you can configure an additional build steps that invoke the synchronization and test result publishing process.

The exact build steps and their order might be dependent on the project context, but a build pipeline that includes synchronization of the scenarios and publishes test results to Azure DevOps, usually follow the structure below (SpecSync specific steps are highlighted):

1. Initialize project (get sources, restore packages, etc.)
2. Build project (compile, build and publish your code)
3. Perform core tests (e.g. unit tests or other programmer tests)
4. **SpecSync push — synchronize scenarios to Azure DevOps Test Cases**
5. Run BDD tests and produce test result file (e.g. run SpecFlow tests)
6. **SpecSync publish-test-results — publish the test results from the test result file to the synchronized Test Cases**

In this documentation we first show how to add a build step to your pipeline that invokes SpecFlow and how to configure the authentication.

In the last sections ([Performing synchronization (push) from build or release pipeline](synchronizing-test-cases-from-build.md#performing-synchronization-push-from-build-or-release-pipeline) and [Publishing test results from build or release pipeline](synchronizing-test-cases-from-build.md#publishing-test-results-from-build-or-release-pipeline)) we show how to configure the SpecSync step for performing `push` and `publish-test-results` commands.

{% hint style="info" %}
This page demonstrates SpecSync usage on CI/CD pipelines using the Azure DevOps build and release pipeline solution. Based on this SpecSync can also be configured on other pipeline solutions (e.g. Jenkins or Bamboo).
{% endhint %}

## Adding SpecSync steps to your build or release pipeline

The SpecSync commands can be added to an Azure DevOps build or release pipeline as a [Command line task](https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/utility/command-line?view=azure-devops\&tabs=yaml) or as a [.NET Core CLI task](https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/build/dotnet-core-cli?view=azure-devops) if you have installed SpecSync as a [.NET Core tool](../installation/dotnet-core-tool.md). These tasks work both in Classic or YAML pipelines. See details on how to configure these tasks below.


{% hint style="info" %}
In order to diagnose synchronization issues, it is recommended to specify a log file for SpecSync execution. This can be done by specifying a `--log $(build.artifactstagingdirectory)\<log-file-name>.log` SpecSync [command line option](../reference/command-line-reference/#common-command-line-options).
{% endhint %}

### Adding a .NET Core CLI task to invoke SpecSync

{% hint style="warning" %}
This option can only be used if you have installed SpecSync as a [.NET Core tool](../installation/dotnet-core-tool.md). in all other cases, check [Synchronizing test cases from build](synchronizing-test-cases-from-build.md#adding-a-command-line-task-to-invoke-specsync) below.
{% endhint %}

If SpecSync has installed as a .NET Core tool, it can be invoked easily from a pipeline using the integrated .NET Core CLI task. The .NET Core tool commands can also be invoked from a Command line task as well of course.

In order to use any .NET Core tool, the tools have to be restored. If SpecSync is your first .NET Core tool in the project, than you need to add a step to do that to the beginning of the pipeline (usually before the normal `dotnet restore` command), as below.

{% tabs %}
{% tab title="Classic pipeline" %}
![](<../.gitbook/assets/image (11).png>)
{% endtab %}

{% tab title="YAML pipeline" %}
```bash
- task: DotNetCoreCLI@2
  displayName: 'dotnet tool restore'
  inputs:
    command: custom
    custom: tool
    arguments: restore
```
{% endtab %}
{% endtabs %}

In order to invoke SpecSync using a the .NET Core CLI command, you have to set the _Command_ proeprty to `custom` , the _Custom command_ property to `specsync` and you need to specify the SpecSync command line arguments (e.g. `push`) in the _Arguments_ property.

The following example shows how to invoke SpecSync `push` command from a Command line tool task from a restored NuGet package. See the following sections for details about the recommended authentication options and for the usual settings for `push `and `publish-test-results` commands.

{% tabs %}
{% tab title="Classic pipeline" %}
![](<../.gitbook/assets/image (13).png>)
{% endtab %}

{% tab title="YAML pipeline" %}
```bash
- task: DotNetCoreCLI@2
  displayName: 'SpecSync push'
  inputs:
    command: custom
    custom: specsync
    arguments: 'push --disableLocalChanges --user "$(SPECSYNC_PAT)"'
    workingDirectory: src/Tests/MyProject.Specs
```
{% endtab %}
{% endtabs %}

### Adding a Command line task to invoke SpecSync

In the SpecSync command line task you need to invoke the `SpecSync4AzureDevOps.exe` executable with the necessary command line parameters. For that you need to ensure that SpecSync has been downloaded to the build agent.&#x20;


If you have installed SpecSync as a [.NET Console App](../installation/dotnet-console.md) by adding a reference to the `SpecSync.AzureDevOps.Console` NuGet package in one of your projects, the build has probably restored this package already. You just need to figure out where the NuGet packages are downloaded. With the usual setup it is either the `packages` folder of your solution for older projects or the folder `$(UserProfile)\.nuget\packages` for newer, SDK-stype projects.

If you use the SpecSync native binaries, you have to make sure that the necessary binaries are downloaded and invoke the SpecSync command line tool from the downloaded location.

If you use the SpecSync official Docker image, you have to invoke the appropriate Docker commands from the command line task.

The following example shows how to invoke SpecSync `push` command from a Command line tool task from a restored NuGet package. See the following sections for details about the recommended authentication options and for the usual settings for `push `and `publish-test-results` commands.

{% tabs %}
{% tab title="Classic pipeline" %}
![](<../.gitbook/assets/image (14).png>)
{% endtab %}

{% tab title="YAML pipeline" %}
```bash
- script: '$(UserProfile)\.nuget\packages\SpecSync.AzureDevOps.Console\3.0.2\tools\SpecSync4AzureDevOps.exe push --disableLocalChanges --user "$(SPECSYNC_PAT)"'
  workingDirectory: src/Tests/MyProject.Specs
  displayName: 'SpecSync push'
```
{% endtab %}
{% endtabs %}

## Authentication settings to perform SpecSync commands from build or release pipeline

For [authentication](../features/general-features/server-authentication-options.md), you can use the built-in [Job access token](../features/general-features/server-authentication-options.md#vsts-alternate-authentication-credentials) or you need an Azure DevOps user account with sufficient privileges (modify test cases and test suites).&#x20;

### Use job access token

Probably the easiest and the simplest is to use the Job access token, that is a security token that is dynamically generated by Azure Pipelines for each job at run time. Learn more about job access tokens in the [Azure DevOps documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/access-tokens?view=azure-devops\&tabs=yaml).

{% hint style="info" %}
To be able to use the Job access token, you need to enable it for the pipeline, by selecting the agent job and in the _Additional options_ section, enable the setting _Allow scripts to access the OAuth token_.

Once the Job access token is enabled, you can refer to it with `$(System.AccessToken)`.
{% endhint %}

Once the job access token is enabled, you can pass its value to SpecSync using the `--user` command line option.

```bash
... push --disableLocalChanges --user "$(System.AccessToken)"
```

### Use a user account for the synchronization

In this section we show how the authentication can be configured using [Personal Access Tokens (PAT)](../features/general-features/server-authentication-options.md#vsts-personal-access-tokens), but the authentication can also be performed with user name and password in a similar way.

#### Step 1: Define a secret environment variable for the PAT in your pipeline

Add a new variable in the Variables section of your pipeline (e.g. `SPECSYNC_PAT`), specify the PAT of the account and make it secret.

![](<../.gitbook/assets/image (15).png>)

#### Step 2: Use the environment variable in SpecSync commands

Once the variable is defined, you can pass its value to SpecSync using the `--user` command line option.

```bash
... push --disableLocalChanges --user "$(SPECSYNC_PAT)"
```

{% hint style="info" %}
_Note: Secret variables can only be used using the `$(variable)` form, they will not be substituted using the %variable% form. _
{% endhint %}

## Performing synchronization (push) from build or release pipeline

The SpecSync push command is usually performed after the successful execution of core tests (e.g. unit tests), but before executing the BDD tests. This way you can ensure that the test cases are updated even if some of the BDD scenarios fail (especially when automated as integration or end-to-end test).

{% hint style="info" %}
Adding the push step to the pipeline ensures that the Test Cases are updated to the exact same version that was used to execute the tests. If the changes have been synchronized locally then this step will just simply detect that the test cases are up-to-date and not change them.
{% endhint %}

The following table contains the settings that are important or usually configured for the `push` command.

| Setting | Description |
| ------- | ----------- |
| Working Directory | For the easiest configuration it is recommended to set the Working Directory property of the task to the folder where your feature-set is (e.g. to the SpecFlow project folder). This is typically the same folder where your [SpecSync configuration file](../features/general-features/configuration-file.md) is located. |
| `--user` | The `--user` command line option can be used so specify the user credentials (e.g. Personal Access Token, PAT) for the user that needs to be used for the synchronization. See [Synchronizing test cases from build](synchronizing-test-cases-from-build.md#authentication-settings-to-perform-specsync-commands-from-build-or-release-pipeline) for details. |
| `--disableLocalChanges` | Creating the initial link between scenarios and test cases requires a small change in the feature file (to include the test case tags). Such changes cannot be done in an automated process, because the changes cannot be committed to the source control. To handle this, SpecSync provides a `--disableLocalChanges` switch (see [command line reference](../reference/command-line-reference/push-command.md) for more details). If this switch is provided, SpecSync will not synchronize new scenarios, but only updates the ones that are already linked to test cases. |
| Task conditions (_Run this task_ property) | The synchronization usually should be performed only for normal build executions and not for Pull Requests. This can be ensured by setting `and(succeeded(), ne(variables['Build.Reason'], 'PullRequest'))` as custom condition (see example below). |

The following example shows a fully configured step that performs the SpecSync push command using the .NET Core CLI task.

{% tabs %}
{% tab title="Classic pipeline" %}
![](<../.gitbook/assets/image (17).png>)
{% endtab %}

{% tab title="YAML pipeline" %}
```bash
- task: DotNetCoreCLI@2
  displayName: 'SpecSync push'
  inputs:
    command: custom
    custom: specsync
    arguments: 'push --disableLocalChanges --user "$(SPECSYNC_PAT)"'
    workingDirectory: src/Tests/MyProject.Specs
  condition: and(succeeded(), ne(variables['Build.Reason'], 'PullRequest'))
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
For information on how to configure the build executing automated test cases, please check the [Synchronizing automated test cases](synchronizing-automated-test-cases.md) article.&#x20;
{% endhint %}


## Publishing test results from a pipeline

To be able to track the test execution results at the test cases, you can execute the tests like usual, save the test result to a test result file (in case of .NET this is a TRX file) and publish the test results using the SpecSync `publish-test-results` command.&#x20;

{% hint style="info" %}
For information on how to configure the build executing automated test cases, please check the [Synchronizing automated test cases](synchronizing-automated-test-cases.md) article.&#x20;
{% endhint %}

### Step 1: Prepare test execution task

In order to publish the test results, the test execution task has to be configured to save the results to a test result file (in case of .NET this is a TRX file). The exact way of doing that depends on the platform and the test execution tool you use. For .NET projects usually the .NET Core CLI task or the VSTest task is used.

If the **.NET Core CLI task** is used, the Arguments property has to be extended with an `--logger trx;logfilename=<your-trx-file-name>.trx` option. The following examples show how to configure the .NET Core CLI task to save the test results to a file `bddestresults.trx`. &#x20;

{% tabs %}
{% tab title="Classic pipeline" %}
![](<../.gitbook/assets/image (27).png>)
{% endtab %}

{% tab title="YAML pipeline" %}
```bash
- task: DotNetCoreCLI@2
  displayName: 'Run BDD Tests'
  inputs:
    command: test
    projects: 'src/Tests/MyProject.Specs/*.csproj'
    arguments: '--configuration $(BuildConfiguration) --logger trx;logfilename=bddtestresults.trx --results-directory $(Agent.TempDirectory)'
    publishTestResults: false
    testRunTitle: 'BDD Tests'
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
The .NET Core CLI task automatically changes the output folder used for the test result file depending on its own "Publish test results and code coverage" (`publishTestResults`) setting, that should normally be disabled when you publish the test results with SpecSync (see below). If the setting is enabled, the output folder is automatically set to the `Agent.TempDirectory)` folder.&#x20;

To remain consistent with this behavior, it is recommended to set the output folder to the same, even if the publish setting is disabled (which is the recommended way). You can achieve that by providing an additional `--results-directory $(Agent.TempDirectory)` argument as you can see in the example above.
{% endhint %}

{% hint style="warning" %}
The .NET Core task with the "test" command by default has the "Publish test results and code coverage" setting enabled (this is the default if you use YAML), but the test results will be published by SpecSync anyway so this setting is unnecessary.

Having the results published both by the .NET Core task and SpecSync might cause the tests appearing twice in the "Tests" tab of the build pipeline. To avoid that, it is recomended to uncheck the "Publish test results and code coverage" setting or specify `publishTestResults: false` in YAML.

As the "Tests" tab only displays published test results if they are marked as "Automated", SpecSync will try to publish the test results as automated even if you haven't enabled the [Mark Test Cases as Automated](../features/push-features/mark-test-cases-as-automated.md) feature for the synchronized Test Cases. In older version of Azure DevOps this is not possible, so you should set it manually if the tests don't appear in the "Tests" tab of the build pipeline.
{% endhint %}


#### Using the VSTest task to execute the tests <a href="using-vstest" id="using-vstest"></a>

The **VSTest task** (used for older .NET Framework projects and for special cases) can be configured similarly. For that task, the test result file can be configured by setting the _Other console options_ property to `/logger:trx;LogFileName=bddtestresults.trx`. You might also need to review the _Test results folder_ property. 

The VSTest task always publishes the test results, but these test results will not be associated to the Test Cases. When you use VSTest and SpecSync test result publishing together, you might see the test results twice at the pipeline result page. If this is causes a problem, you can use the `VsTestForSpecSync` task instead of the VSTest task (see details [here](how-to-use-the-pipeline-tasks.md)) or [disable the pipeline association](../features/test-result-publishing-features/publishing-test-result-files.md#test-results-can-be-associated-to-an-azure-devops-build) feature of SpecSync.

### Step 2: Configure SpecSync task to publish test result file

In order to publish the test results, a SpecSync task has to be added right after the test execution task. See [Adding SpecSync steps to your build or release pipeline](synchronizing-test-cases-from-build.md#adding-specsync-steps-to-your-build-or-release-pipeline) for details about how to do this.

The following table contains the settings that are important or usually configured for the `publish-test-results` command.

| Setting | Description |
| ------- | ----------- |
| Working Directory | For the easiest configuration it is recommended to set the Working Directory property of the task to the folder where your feature-set is (e.g. to the SpecFlow project folder). This is typically the same folder where your [SpecSync configuration file](../features/general-features/configuration-file.md) is located. |
| `--user` | The `--user` command line option can be used so specify the user credentials (e.g. Personal Access Token, PAT) for the user that needs to be used for the synchronization. See [Synchronizing test cases from build](synchronizing-test-cases-from-build.md#authentication-settings-to-perform-specsync-commands-from-build-or-release-pipeline) for details. |
| `--testConfiguration` | When publishing test results to Azure DevOps Test Cases (Test Suites), you have to select a Test Configuration that is associated to the Test Suite of your Test Cases. You can specify the Test Configuration in the configuration file or using the `--testConfiguration` command line option. |
| `--testResultFile` | The test results file produced by the test execution step can be specified using the `--testResultFile` option. Make sure you specify the file from the right folder (usually `$(Agent.TempDirectory)`). |
| `--testResultFileFormat` | This setting is needed if the test result file is NOT a TRX file. For TRX files this setting can be omitted. The possible format values are listed in the [Compatibility](../reference/compatibility.md#supported-test-result-formats) page. |
| `--runName` | There are many settings that can be used to customize the Test Run created by SpecSync. You can find these settings in the [publish-test-results](../reference/command-line-reference/publish-test-results-command.md) page. The `--runName` setting for example can be used to specify a name of your Test Run, so that it can be easily distinguished from the normal test execution results.                                                                                                                   |
| Task conditions (_Run this task_ property) | <p>It is important that the publish-test-results command is performed even if the test execution failed. By default the tasks are only executed if all previous tasks succeeded. Also make sense to note that test result publishing usually should be performed only for normal build executions and not for Pull Requests. </p><p>This two conditions can be ensured by setting <code>and(succeededOrFailed(), ne(variables['Build.Reason'], 'PullRequest'))</code> as custom condition (see example below).</p> |

The following example shows a fully configured step that performs the SpecSync `publish-test-results` command using the .NET Core CLI task. The example assumes that the test results were saved to a file `bddtestresults.trx` in the folder `$(Agent.TempDirectory)` like it was configured in the example of the previous step.

{% tabs %}
{% tab title="Classic pipeline" %}
![](<../.gitbook/assets/image (21).png>)
{% endtab %}

{% tab title="YAML pipeline" %}
```bash
- task: DotNetCoreCLI@2
  displayName: 'SpecSync publish-test-results'
  inputs:
    command: custom
    custom: specsync
    arguments: 'publish-test-results --user "$(specSyncPAT)" --testConfiguration "Windows 10" --testResultFile $(Agent.TempDirectory)\bddtestresults.trx --runName "BDD Tests"'
    workingDirectory: src/Tests/MyProject.Specs
  condition: and(succeededOrFailed(), ne(variables['Build.Reason'], 'PullRequest'))

```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
In Azure DevOps the pipeline can only be associated to the created Test Run, if that pipeline is in the same Azure DevOps project as the synchronized Test Cases. If you try to publish test result from a different project, you will receive a **pipeline not found error**. See the [Troubleshooting guide](../contact/troubleshooting.md#pipeline-not-found) for a workaround to this limitation.
{% endhint %}
