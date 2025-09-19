# Publishing test result files

In order to turn the Test Cases in Azure DevOps into a real living documentation it is essential to register the execution results of the Test Cases. Having the Test Case execution results registered in Azure DevOps might be also important as part of the traceability requirements.

![Execution results linked to a Test Case synchronized from a scenario](<../../.gitbook/assets/image (23).png>)

You can publish scenario execution results (test results) to Azure DevOps with SpecSync using the `publish-test-result` command that works with many BDD tool and platform including SpecFlow, Reqnroll, Cucumber and PyTest (Check [Compatibility](../../reference/compatibility.md#supported-test-result-formats) page for the full list).

![Test execution result with iteration and step execution details](<../../.gitbook/assets/image (24).png>)

This documentation page describes in detail the concept for publishing test results with SpecSync. The complete reference guide of the command line options can be found in the [publish-test-results reference](../../reference/command-line-reference/publish-test-results-command.md) page. The configuration settings for test result publishing are in the [publishTestResults configuration section](../../reference/configuration/configuration-publishtestresults.md).

The [Examples](publishing-test-result-files.md#examples) section below shows a few concrete examples. A simple usage of the command could be as simple as this:

```
dotnet specsync publish-test-results --testResultFile result.trx
```

The [Use SpecSync from build or release pipeline](../../important-concepts/synchronizing-test-cases-from-build.md) guide contains details about how to integrate the publish-test-results command to a build or release pipeline.

{% hint style="info" %}
SpecSync operations, including the publish-test-results command supports "dry-run" mode using the `--dryRun` command line option. In dry-run mode, no change is made neither to Azure DevOps nor to the feature files, so you can test the impact of an operation without making an actual change.
{% endhint %}

{% hint style="info" %}
You can find an overview of the Azure DevOps plans in the [Azure DevOps documentation](https://docs.microsoft.com/en-us/azure/devops/test/navigate-test-plans?view=azure-devops). The [Track test status page in the Azure DevOps documentation](https://learn.microsoft.com/en-us/azure/devops/test/track-test-status?view=azure-devops) shows the different reports and charts you can create for the results.
{% endhint %}

## Getting scenario execution result from your BDD tool

The automation source of the Test Cases are provided by the BDD scenarios and the automation solution implemented for them. The automation solution might use different tools (SpecFlow, Reqnroll, Cucumber, PyTest, etc.) and might run on different platforms (e.g. .NET, Java or Python).

SpecSync uses a concept that works with different tools and platforms: execute the scenarios as usual with their standard tooling and save the test results into a test result file of their native test result format. SpecSync understands many test result file formats and how they represent the scenario results and able to match them to the Test Cases, synchronized earlier with a push command.

The [Compatibility](../../reference/compatibility.md#supported-test-result-formats) page contains the currently supported BDD tools and test result file formats. Please [contact us](../../contact/specsync-support.md) if your tool is not in the list and we are happy to provide support for that. You can also support custom test result file formats with a [SpecSync plugin](../general-features/specsync-plugins.md).

The exact way how you can get test result files depends on the test runner tool and the BDD framework you use. For example for .NET Reqnroll test projects the test results can be saved into a TRX file by providing the `--logger trx;logfilename=<your-trx-file-name>.trx` option for the `dotnet test` command.

For the SpecSync publish-test-results command, the path of the test result file can be specified using the `--testResultFile` command line option. If the test result file is not a TRX file, you also have to specify the file format using the `--testResultFileFormat` option (see [Compatibility](../../reference/compatibility.md#supported-test-result-formats) for possible values).

The [Examples](publishing-test-result-files.md#examples) section below shows how some of the most commonly used BDD frameworks and platforms can be used.

## Publish test results to Azure DevOps

Azure DevOps records test results for Test Cases in a flexible way and to be able to understand how SpecSync needs to be configured it makes sense to review the most important aspects of the Azure DevOps test result behavior and their implication to the SpecSync synchronization process.

{% hint style="info" %}
Internally Azure DevOps uses a concept of a Test Point, that basically represents a slot to record test results. A test point is a combination of a Test Case, a Test Suite and a Test Configuration. So basically to be able to record a test result for a Test Case, you also need to specify a Test Suite and a Test Configuration as well.&#x20;
{% endhint %}

{% hint style="info" %}
A set of test results are grouped together into a Test Run. A Test Run can contain multiple test results even for different Test Configurations and Test Suites as well, but the Test Suites must belong to the same Test Plan.
{% endhint %}

### Test results belong to a Test Configuration

The test results in Azure DevOps are always connected to a Test Configuration, so you can choose a Test Configuration when publishing the test results with SpecSync.&#x20;

{% hint style="info" %}
SpecSync automatically chooses the Test Configuration when it is not specified and the target Test Suite has only one configuration assigned.
{% endhint %}

The Test Configuration represents the configuration your product was tested with. A configuration can be the target operating system, the target browser or the target mobile phone, but many teams do not need to represent these configuration differences  in BDD scenario executions. Depending on your needs, you can configure multiple Test Configurations in Azure DevOps (see [documentation](https://docs.microsoft.com/en-us/azure/devops/test/test-different-configurations?view=azure-devops)) or just create a default configuration. Azure DevOps always creates a default Test Configuration called `Windows 10` (can be renamed).

In case you you do not plan to publish results to multiple configurations, you can omit the Test Configuration setting or specify it in the config file by providing either the [`publishTestResults/testConfiguration/name` or `publishTestResults/testConfiguration/id`](../../reference/configuration/configuration-publishtestresults.md) setting.

In case you publish results to different Test Configurations it is better to specify it using the [`--testConfiguration` command line option](../../reference/command-line-reference/publish-test-results-command.md).

### Test results belong to a Test Suite

Test results in Azure DevOps always belong to a Test Suite and you cannot record test results to Test Cases that are not included in any Test Suites.&#x20;

For SpecSync this means you have to select a Test Suite to publish the results for. By default SpecSync uses the Test Suite configured for synchronizing the test cases (see [Include synchronized Test Cases to a Test Suite](../common-synchronization-features/group-synchronized-test-cases-to-a-test-suite.md)), but you can also choose different Test Suite as well using the [`publishTestResults/testSuite/name` or `publishTestResults/testSuite/id`](../../reference/configuration/configuration-publishtestresults.md) setting or the [`--testSuite` command line option](../../reference/command-line-reference/publish-test-results-command.md). There is no restriction for the Test Suite type, you can specify static, query-based and requirement-based Test Suites as well.

{% hint style="info" %}
Since you can specify the Test Suite for publishing, the publish-test-results command works even if you have not configured SpecSync to [include Test Cases to a Test Suite](../common-synchronization-features/group-synchronized-test-cases-to-a-test-suite.md). But using that feature has other benefits as well so it is recommended to use it even if you publish the results to a different suite.
{% endhint %}

### Results published to a Test Suite are not visible in other Suites

As the results are connected to a single Test Suite, when a Test Case result is published to that Test Suite that result is not visible when you check the same Test Case in a different Test Suite.

Teams usually decide on the Test Suite they use as "living documentation", so the Test Suite that they check if they are interested in the test execution results.

Alternatively you can use the [Customization: Publishing test results to multiple Test Suites](customization-publishing-test-results-to-multiple-test-suite.md) feature of SpecSync, that allows you to publish the same test result to multiple Test Suites (even into all Test Suite where the Test Case is included). Because of Azure DevOps limitations, these Test Suites must belong the the same Test Plan.

### Test results are grouped into a Test Run

Azure DevOps groups the test results together into a Test Run. The list of Test Runs can be found in the _Test Plans / Runs_ section of the Azure DevOps project web portal.&#x20;

The SpecSync publish-test-results command always creates a single Test Run. You can customize many details of the created Test Run using the [command line](../../reference/command-line-reference/publish-test-results-command.md) and the [configuration](../../reference/configuration/configuration-publishtestresults.md) options. For example you can set the name of the Test Run using the `--runName` option.

The Azure DevOps Test Runs have a _Run Type_ setting that can either be _Automated_ or _Manual_. SpecSync sets this flag by default based on whether it is configured to [synchronize scenarios as automated test cases](../push-features/mark-test-cases-as-automated.md) (`synchronization/automation/enabled` is set to `true`). The value can be overwritten by setting `publishTestResults/runType`.&#x20;

{% hint style="info" %}
Inconsistent setting of the Test Case automation flag and the Test Run run type might lead to "Mismatch in automation status of test case and test run" error in older Azure DevOps servers. See related [Troubleshooting entry](../../contact/troubleshooting.md#test-result-publishing-fails-with-mismatch-in-automation-status-of-test-case-and-test-run) for details.
{% endhint %}

SpecSync always attaches the test result file to the created Test Run, but you can attach additional files using the `--attachedFiles` option.

### Test results can be associated to an Azure DevOps build

The test results can also be associated with a build (a concrete run of a build pipeline) to track the execution source.&#x20;

When the SpecSync publish-test-results command is invoked from an Azure DevOps build pipeline, it will detect the ID of the currently executing build and automatically associates the results to that build. For other situations the build can also be associated using the `--buildId` or the `--buildNumber` command line options. To disable the automatic build association, you have to specify the `--disablePipelineAssociation` option (or set the `--buildId` option to an empty value before v3.3.3). For more information about using SpecSync from a pipeline, please check the [How to use SpecSync from build or release pipeline](../../important-concepts/synchronizing-test-cases-from-build.md) guide.

{% hint style="info" %}
In Azure DevOps only Azure DevOps build pipelines can be associated. Build references for other build servers (e.g. Jenkins) can be recorded using the Test Run comment or the individual test result comments with the `--runComment` or the `--testResultComment` command line options.
{% endhint %}

{% hint style="warning" %}
In Azure DevOps the pipeline can only be associated to the created Test Run, if that pipeline is in the same Azure DevOps project as the synchronized Test Cases. If you try to publish test result from a different project, you will receive a **pipeline not found error**. See the [Troubleshooting guide](../../contact/troubleshooting.md#pipeline-not-found) for a workaround to this limitation.
{% endhint %}

### Results of Scenario Outline executions

Scenario outlines represent data-driven variation tests: the same test is executed with different input data. SpecSync [synchronizes scenario outlines to parametrized Test Cases](../push-features/synchronizing-scenario-outlines.md).&#x20;

SpecSync publishes the result of a scenario outline is cumulated from the results of the individual data variations (examples). In this cases the test result comment contains the individual results or the error messages. SpecSync also publishes these results as sub-results of the test result, but currently sub-results are not displayed in the Azure DevOps user interface (they can be retrieved though the API though). To check these results in detail, you need to open the test result file attached to the Test Run.

### Publishing inconclusive test results

Some test execution frameworks report skipped scenarios as Inconclusive. Publishing them as inconclusive result (that is a kind of failure) would make the overall test outcome and the detail statistics invalid.

My setting the `publishTestResults/treatInconclusiveAs` setting in the configuration file, you can map this result to another value, e.g. `NotExecuted`.

### Merging multiple test result files

You can also specify multiple test result files. In this case SpecSync will merge the results and publish them as a single Test Run.

The `--testResultFile` command line option allows specifying multiple files, separated by a semicolon (`;`).

Multiple test results files can also be specified by specifying a folder name. In this case SpecSync will scan through the folder and uses all files that are supported by the specified format setting (e.g. all TRX files).

### Custom Test Run and Test Result settings

The Test Runs and the individual Test Results within that can have additional settings (e.g. comment) that can be specified. These settings can be specified from the [command line](../../reference/command-line-reference/publish-test-results-command.md) (e.g. `--runComment`) or using the `publishTestResults/testRunSettings` and `publishTestResults/testResultSettings` [configuration settings](../../reference/configuration/configuration-publishtestresults.md).

In the specified values, you can also use different placeholders, e.g. the `{br}` placeholder to include a new line. For the complete list of placeholders that can be used, please check the [reference](../../reference/configuration/configuration-publishtestresults.md#setting-placeholders).

## Detecting test reruns and flaky tests

SpecSync can automatically detect when tests have been rerun multiple times and determine the overall outcome. This is particularly useful when working with test runners that support automatic test reruns for failed tests, such as VSTest with the "rerun failed tests" option. 

If the test is identified as a "flaky test" (tests that were rerun multiple times and at least one attempt failed), the overall outcome is determined based on configurable behavior. The behavior can be controlled using the [`publishTestResults/flakyTestOutcome`](../../reference/configuration/configuration-publishtestresults.md) configuration setting, by default it uses the last rerun attempt as the overall result.

For more details about the configuration options, see the [flakyTestOutcome setting](../../reference/configuration/configuration-publishtestresults.md) in the configuration reference.

## Examples

### SpecFlow (.NET Core)

The following example shows how to run .NET Core SpecFlow tests using the `dotnet test` command and publish the test result file (`bddtestresults.trx`) to the default Test Configuration. The Test Suite in this case is the Test Suite that the scenarios are synchronized to (`BDD Scenarios`).

{% code title="specsync.json" %}
```javascript
{
  "$schema": "http://schemas.specsolutions.eu/specsync4azuredevops-config-latest.json",

  "remote": {
    "projectUrl": "https://specsyncdemo.visualstudio.com/MyCalculator",
    "testSuite": {
      "name": "BDD Scenarios"
    }
  }
}
```
{% endcode %}

```
dotnet test --logger trx;logfilename=bddtestresults.trx
dotnet specsync publish-test-results --testResultFile bddtestresults.trx
```

### Cucumber (Java, Maven)

The following example shows how to run Cucumber Java tests using Maven and publish the test result file to the default Test Configuration.&#x20;

When running `mvn test` the test result file is saved to a file name that matches to your package name under the `target/surefire-reports` folder, e.g. `target/surefire-reports/TEST-eu.specsolutions.calculator.CucumberTest.xml`. Check the [Maven Surefire Report Plugin documentation](https://maven.apache.org/surefire/maven-surefire-report-plugin/examples/report-custom-location.html) for details.

The Test Suite in this case is the Test Suite that the scenarios are synchronized to (`BDD Scenarios`). In this example we use the [SpecSync native binaries](../../installation/native-binaries.md) to invoke the command, but the same would work with the other [installation options](../../installation/) too.

{% code title="specsync.json" %}
```javascript
{
  "$schema": "http://schemas.specsolutions.eu/specsync4azuredevops-config-latest.json",

  "remote": {
    "projectUrl": "https://specsyncdemo.visualstudio.com/MyCalculator",
    "testSuite": {
      "name": "BDD Scenarios"
    }
  }
}
```
{% endcode %}

```
mvn test
<SPECSYNC-FOLDER>/SpecSync4AzureDevOps publish-test-results --testResultFile bddtestresults.xml --testResultFileFormat cucumberJavaJUnitXml
```

### Publish using a specific Test Configuration

The following example shows how to publish the results to a specific Test Configuration. In this case the tests are executed both with Chrome and Firefox, therefore we created two Test Configurations (`Chrome` and `Firefox`) and assigned them to the Test Suite that the scenarios are synchronized to (`BDD Scenarios`).

We executed the tests for Chrome and saved the results to a file `bddresults-chrome.trx`. After that publishing the results to the Chrome configuration can be done with

```
dotnet specsync publish-test-results -r bddresults-chrome.trx --testConfiguration "Chrome"
```


### SpecFlow (.NET Framework, VSTest)

The following example shows how to run .NET Framework SpecFlow tests using the `vstest.console` command and publish the test result file (`bddtestresults.trx`) to the default `Windows 10` Test Configuration. The runner saves the test result file to the `TestResults` folder by default. (.NET Framework tests can also be executed using the `dotnet test` command, see [above](publishing-test-result-files.md#specflow-net-core).)&#x20;

The Test Suite in this case is the Test Suite that the scenarios are synchronized to (`BDD Scenarios`). In this example we use the [SpecSync .NET Console App](../../installation/dotnet-console.md) to invoke the command, but the same would work with the other [installation options](../../installation/) too.

{% code title="specsync.json" %}
```javascript
{
  "$schema": "http://schemas.specsolutions.eu/specsync4azuredevops-config-latest.json",

  "remote": {
    "projectUrl": "https://specsyncdemo.visualstudio.com/MyCalculator",
    "testSuite": {
      "name": "BDD Scenarios"
    }
  }
}
```
{% endcode %}

```
vstest.console.exe /logger:trx;LogFileName=bddtestresults.trx
dotnet specsync publish-test-results --testResultFile TestResults\bddtestresults.trx --testConfiguration "Windows 10"
```

### Custom publish settings

The following example shows how to customize the settings for the publish-test-results command. (We use a SpecFlow .NET Core result file `bddresults.trx` as an example here.) The following customizations are applied:

* The Test Configuration is specified in the config file (instead of command line)
* The results are published to a Test Suite (`BDD Results`, in Test Plan ID `23`) different from the one where the scenarios are synchronized to
* The name of the Test Run has been specified

{% code title="specsync.json" %}
```javascript
{
  "$schema": "http://schemas.specsolutions.eu/specsync4azuredevops-config-latest.json",

  "remote": {
    "projectUrl": "https://specsyncdemo.visualstudio.com/MyCalculator",
    "testSuite": {
      "name": "BDD Scenarios"
    }
  },
  "publishTestResults": {
    "testConfiguration": {
      "name": "Chrome"
    },
    "testSuite": {
      "name": "BDD Results",
      "testPlanId": 23  // optional, but makes processing faster
    }
  }
}
```
{% endcode %}

```
dotnet specsync publish-test-results --testResultFile bddtestresults.trx --runName "BDD Results"
```
