# Synchronizing test cases from build

Keeping the test cases in sync with the scenarios is important, therefore automating the synchronization process is recommended. This can be done for example from Azure DevOps build or release pipeline: you can configure an additional build step that invokes the synchronization process.

### Adding SpecSync steps to your build or release pipeline

The SpecSync commands can be added to an Azure DevOps build or release pipeline as a [Command line task](https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/utility/command-line?view=azure-devops&tabs=yaml). This works for both in Classic or YAML pipelines.

In the SpecSync command line task you need to invoke the `SpecSync4AzureDevOps.exe` executable with the necessary command line parameters usually from the folder of your SpecFlow project. For invoking the executable you can use the path of the restored NuGet package or the `specsync4azuredevops.cmd` script. \(See more at [Usage](../reference/command-line-reference/).\)

See the following sections for details about the recommended authentication options and for the usual settings for `push` and `publish-test-results` commands.

### Authentication settings to perform SpecSync commands from build or release pipeline

For [authentication](tfs-authentication-options.md), it is recommended to create a special user account that has sufficient privileges \(modify test cases and test suites\). The user name and password for the special account can be specified to the build task as an environment variable. When specifying the user name and password \(both in the configuration file or as a command line option\), you can use environment variables in the `%variable%`  or `$(variable)` form. It is recommended to define these variables as secret variables \(see related [Azure DevOps documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/variables)\). Secret variables can only be used using the `$(variable)` form.

For authentication, the recommended way is to use [Personal Access Tokens \(PAT\)](tfs-authentication-options.md#vsts-personal-access-tokens) that has to be specified for SpecSync as "user".

An example configuration setting in the SpecSync configuration file that uses the `SPECSYNC_PAT` environment variable:

```javascript
{
  ...
 "remote": {
    "projectUrl": "https://dev.azure.com/myorganization/MyProject",
    "user": "%SPECSYNC_PAT%"
  },
  ...
}
```

{% hint style="info" %}
Note: Secret variables can only be used from the configuration file if they are mapped to an environment variable first \(only available for YAML pipelines\). Please check [Azure DevoOps documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&tabs=yaml%2Cbatch#secret-variables) for details. For using secret variables in Classic pipelines, we recommend overriding the user setting from command line as shown below.
{% endhint %}

The same setting can also be alternatively specified as a command line option:

```bash
specsync4azuredevops.cmd push --buildServerMode --user "$(SPECSYNC_PAT)"
```

{% hint style="info" %}
_Note: Secret variables can only be used using the `$(variable)` form, they will not be substituted using the %variable% form._ 
{% endhint %}

### Performing synchronization \(push\) from build or release pipeline

Creating the initial link between scenarios and test cases requires a small change in the feature file \(to include the test case tags\). Such changes cannot be done in an automated process, because the changes cannot be committed to the source control. To handle this, SpecSync provides a `--buildServerMode` switch \(see [Usage](../reference/command-line-reference/) for more details\). If this switch is provided, SpecSync will not synchronize new scenarios, but only updates the ones that are already linked to test cases.

{% hint style="success" %}
It is recommended to add the SpecSync push command after the build and before executing the automated tests. This way you can ensure that the test cases are updated even if some of the test \(especially integration tests\) fail.
{% endhint %}

![Invoke synchronization from a build task](../.gitbook/assets/build-invoke-synchronization-from-build-task.png)

{% hint style="info" %}
For information on how to configure the build executing automated test cases, please check the [Synchronizing automated test cases](synchronizing-automated-test-cases.md) article. 
{% endhint %}

### Publishing test results from build or release pipeline

To be able to track the test execution results at the test cases from an assembly-based test execution, you can execute the tests from the compiled assembly using the Visual Studio Test \(VSTest\) task and then publish the results \(from the generated TRX file\) to the test cases using the SpecSync `publish-test-results` command. You can find more information about this strategy in the [Synchronizing automated test cases](synchronizing-automated-test-cases.md#assembly-based-execution-strategy) page.

To be able to publish the test results, you need to configure the location of the TRX file and you also need to provide the Test Configuration to record the results for. 

{% hint style="success" %}
The SpecSync publish test result task is usually added to the pipeline right after the assembly-based test execution task. 

To be able to publish the results even if the tests failed, make sure you set the "Run this task" setting within the "Control Options" group to "Even if a previous task has failed, unless the build was cancelled".
{% endhint %}

The following Command Line Task content shows a usual setting.

![A command line task configured for publishing test results with SpecSync](../.gitbook/assets/image%20%282%29.png)

{% hint style="info" %}
You can control the location and the name of the TRX file produced by the VSTest task by specifying the logger results in the "Other console options" setting, for example in order to save the TRX file into `..\TestResults\assembly-testresult.trx`as it was used in the example above, you need to specify:

```text
/logger:trx;LogFileName=assembly-testresult.trx
```
{% endhint %}

















