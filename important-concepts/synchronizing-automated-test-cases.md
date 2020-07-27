# Synchronizing automated test cases

_**Important: This article describes the behavior of SpecSync for Azure DevOps v2.1 or later. This version introduced major improvements for automated test cases. For the v2.0 behavior, please check**_ [_**this article**_](synchronizing-automated-test-cases-specsync-v2.0.md)_**.**_ 

The test cases synchronized by SpecSync can also track test executions and test results through the "Test Run" infrastructure of Azure DevOps. This might be useful to create a "Living Documentation" -- a description of the expected system behavior that can also indicate whether the solution currently fulfills these expectations or not. Tracking the test results for the test cases is optional.

The test results in Azure DevOps are always registered to a _Test Point_. A Test Point is an association of a _Test Case_, a _Test Suite_ containing the Test Case and a _Test Configuration_ associated to the Test Suite.

Besides tracking the test results, the test cases can be marked as "Automated", although test results can also be registered to a non-automated test case as well.

SpecSync supports two main test execution strategies for tracking test case results:

* **Assembly based execution** -- the scenarios are executed from the test assembly locally or in a build/release pipeline and the test results are published to the linked test cases by invoking the SpecSync `publish-test-results` command.
* **Test Suite based execution** -- the automated test cases are executed by Azure DevOps using a Visual Studio Test \(VSTest\) task in a build or release pipeline that is configured to select tests using a Test Plan.

Currently these strategies work only with SpecFlow projects. For non-SpecFlow projects \(e.g. Cucumber projects\), SpecSync can synchronize the scenarios to **non-automated test cases**. The synchronized non-automated test cases can be managed, linked and structured in Azure DevOps. We plan to support the Assembly based execution strategy for non-SpecFlow projects as well, please [contact us](https://www.specsolutions.eu/contact/) if you would like to use this feature.

For an overview of the compatibility of the different execution strategies, please check the following PDF document: [Test Case Execution Strategy Compatibility](http://speclink.me/specsyncexecstcomp)

![Test Case Execution Strategy Compatibility \(download it as PDF from the link above\)](../.gitbook/assets/image%20%286%29.png)

## Assembly based execution strategy

The assembly-based execution strategy is the most generally applicable strategy and it requires the least changes in a usual continuous integration \(CI\) process.

The core concept of the Assembly based execution strategy is that the scenarios are executed on the basis of the compiled test assembly as usual, and the execution results are published by SpecSync in a way that the individual test results will be connected to the Test Case work items synchronized from the scenarios.

### Step 0: Configure SpecSync

In order to publish test results to a test case in Azure DevOps, the test case has to be included to a Test Suite. This can be achieved by configuring the test suite name in the [`remote/testSuite`](../configuration/configuration-remote.md) entry of the configuration file. See [Group synchronized test cases to a test suite](group-synchronized-test-cases-to-a-test-suite.md) for details.

```javascript
{
  ...
  "remote": {
    ...
    "testSuite": {
      "name": "BDD Scenarios"
    }
  },
  ...
}
```

Although for this strategy it is not required, you can synchronize the test cases as automated test cases to indicate their execution type. In this case, you need to specify `assemblyBasedExecution` as `testExecutionStrategy` in the [`synchronization/automation`](../configuration/configuration-synchronization/configuration-synchronization-automation.md) section of the configuration file.

```javascript
{
  ...
  "synchronization": {
    ...
    "automation": {
      "enabled": true,
      "testExecutionStrategy": "assemblyBasedExecution"
    },
    ...
  },
  ...
}
```

### Step 1: Synchronize the scenarios to make sure the test cases are up-to-date

To be able to publish the test results to the appropriate test cases it is recommended to perform a synchronization before executing the tests and publishing the results using the SpecSync `push` command. See [Usage](../reference/usage.md) and [Synchronizing test cases from build](synchronizing-test-cases-from-build.md) for details.

### Step 2: Execute scenarios from test assembly

The scenarios can be executed locally, using the `vstest.console` or the `dotnet test` commands, or in an Azure DevOps build/release pipeline using the Visual Studio Test \(VSTest\) task. In either case, the test execution has to be configured in a way that it saves the test results into a TRX file.

The following examples show how this can be achieved.

```bash
vstest.console bin\Debug\MyTestAssembly.dll  /logger:trx;LogFileName=testresult.trx
```

```bash
dotnet test --logger trx;logfilename=testresult.trx
```

![Configure TRX file in the &quot;Execution options&quot; section of the VSTest taks](../.gitbook/assets/image%20%285%29.png)

{% hint style="info" %}
The TRX files you specify will be saved to a `TestResults` folder. So using the configuration above, the TRX file will be saved to `TestResults\testresult.trx`.
{% endhint %}

{% hint style="info" %}
To be able to publish test results with failing tests in a build/release pipeline, the "Continue on error" setting in the "Control Options" section of the VSTest task has to be enabled.
{% endhint %}

### Step 3: Publish test results using SpecSync

The test results saved into the TRX file can be published by SpecSync using the `publish-test-results` command. The command will match the individual test results to the test cases linked to the scenarios, but the matching can only work if the scenarios have been synchronized recently and if the test cases are also synchronized to a Test Suite \(see Step 1\). 

The publish-test-results command has to be provided with the following settings

* Path of the TRX file generated by the test execution in Step 2
* The Azure DevOps Test Configuration to associate the test results with. 

You can find the available Test Configurations in the "Test Plans / Configuration" section of the Azure DevOps project page, but if you specify a wrong test configuration value, SpecSync will also list the available options.

The TRX file and the Test Configuration can be specified either in the configuration file \([`publishTestResults`](../configuration/publishtestresults.md) section\) or as a command line argument, as the following example shows.

```bash
path-to-specsync-package\tools\SpecSync4AzureDevOps.exe publish-test-results 
  --testConfiguration "Windows 10" --testResultFile TestResults\testresult.trx
```

As a result of the `publish-test-results` command a new Test Run will be registered and the test results will be associated to the test cases as you can see when you open the Test Suite in Azure DevOps.

![Test Cases with results in a Test Suite](../.gitbook/assets/image%20%281%29%20%281%29.png)

## Test Suite based execution strategy

The test suite based execution strategy uses the test case execution infrastructure of Azure DevOps that is exposed through a Visual Studio Test \(VSTest\) task that is configured to select tests using a Test Plan.

Because of this there are a few limitations that have to be considered when planning to use this strategy:

1. The used Azure DevOps infrastructure cannot be used for local execution, therefore the test results can only be published from Azure DevOps build/release pipeline.
2. The used Azure DevOps infrastructure supports only MsTest V1 test frameworks in TFS 2018 or earlier. The support for other test frameworks has been introduced to Azure DevOps recently.
3. The used Azure DevOps infrastructure does not support associating multiple test methods for a parametrized \(data-driven\) Test Case. SpecFlow generates multiple test methods for Scenario Outline when MsTest or SpecFlow+ Runner is used as a unit test provider, therefore to be able to associate a single test method for the test cases, SpecSync provides a SpecFlow Plugin that generates Scenario Outline wrapper methods to be used by this Azure DevOps execution infrastructure. See section [Test Suite based execution strategy with Scenario Outline wrappers](synchronizing-automated-test-cases.md#test-suite-based-execution-with-scenario-outline-wrappers-strategy) for more details about this. To xUnit, limitation does not apply. To NUnit, the Scenario Outline wrappers have to be generated until Azure DevOps fully supports data-driven NUnit test execution.

The core concept of the Test Suite based execution strategy is that the scenarios are synchronized by SpecSync as automated test cases in a Test Suite and executed as part of a build/release pipeline.

### Step 0: Configure SpecSync <a id="suitebasedexecution-step0"></a>

In order to publish test results to a test case in Azure DevOps, the test case has to be included in a Test Suite. This can be achieved by configuring the test suite name in the [`remote/testSuite`](../configuration/configuration-remote.md) entry of the configuration file. See [Group synchronized test cases to a test suite](group-synchronized-test-cases-to-a-test-suite.md) for details.

```javascript
{
  ...
  "remote": {
    ...
    "testSuite": {
      "name": "BDD Scenarios"
    }
  },
  ...
}
```

This strategy requires the synchronization of the scenarios to **automated test cases**. To be able to configure this, you need to

* set `enabled` to `true`
* specify `testSuiteBasedExecution` as `testExecutionStrategy`

in the [`synchronization/automation`](../configuration/configuration-synchronization/configuration-synchronization-automation.md) section of the configuration file.

```javascript
{
  ...
  "synchronization": {
    ...
    "automation": {
      "enabled": true,
      "testExecutionStrategy": "testSuiteBasedExecution"
    },
    ...
  },
  ...
}
```

### Step 1: Synchronize the scenarios to make sure the test cases are up-to-date

In order to be able run the automated test cases, they have to be up-to-date with the automation code. To achieve this, it is recommended to perform a synchronization before executing the tests using the SpecSync `push` command. See [Synchronizing test cases from build](synchronizing-test-cases-from-build.md) for details.

### Step 2: Execute scenarios from Test Suite 

In order to be able to execute the scenarios from Test Suite, a Visual Studio Test \(VSTest\) task has to be added to the Azure DevOps build/release pipeline, which is configured as:

* Select "Test Plan" in the "Select tests using" setting
* Select the Test Plan and the Test Suite that was configured for SpecSync synchronization
* Select a Test Configuration

![A VSTest task configured to execute synchronized test cases](../.gitbook/assets/image.png)

{% hint style="warning" %}
Due to a bug in the VSTest task in Azure DevOps, if the name of the compiled assembly containing the automated scenarios does not contain the word "test" \(e.g. when called as MyProject.Specs.dll\), the execution will fail with "No test assemblies found". This can be fixed by changing back the task to Assembly based, fix the "Source Filter" setting and switch it back to Test Plan based. See related [issue](https://github.com/Microsoft/azure-pipelines-tasks/issues/10006) on GitHub. 
{% endhint %}

During the execution of the build or release, the scenarios will be executed and their results will be registered to the linked Test Cases as you can see when you open the Test Suite in Azure DevOps.

![Test Cases with results in a Test Suite](../.gitbook/assets/image%20%281%29%20%281%29.png)

As a result of the synchronization step, the test cases are marked as "Automated" and the test methods are associated with the test cases.

![Test case in Azure DevOps with associated automation](../.gitbook/assets/getting-started-specflow-associated-automation.png)

{% hint style="info" %}
The steps described in this section will cause test failures to Scenario Outlines when either MsTest or NUnit or SpecFlow+ Runner is used as a unit test provider, as for these providers an additional wrapper method has to be generated. See section Suite based execution strategy with Scenario Outline wrappers for the details about the additional steps required.
{% endhint %}

## Test Suite based execution with Scenario Outline wrappers strategy

The Suite based execution with Scenario Outline wrappers strategy follows the same concept as the Suite based execution strategy, however, it contains the additional configuration steps required for SpecFlow projects using MsTest or NUnit unit test provider. \(With SpecFlow+ Runner, currently only the Assembly based execution strategy is available.\) For projects using legacy MsTest V1 provider, you can also consider the "Use TestCase Data" mode \(see [below](synchronizing-automated-test-cases.md#use-testcase-data-for-scenario-outline-examples-for-legacy-mstest-v1-projects)\).

For the unit test providers that generate multiple test methods for the Scenario Outlines, SpecSync can generate a special wrapper method, which wraps the execution of the individual Scenario Outline examples and can be associated with the automated test case.

{% hint style="info" %}
While the generated wrapper method provides a sufficient solution for the test case execution, it causes a slight inconvenience, because it will be also shown in the test runners used locally and when there is an assembly based test execution included in the build or release pipeline. Therefore for these unit test providers we recommend considering the Assembly based execution strategy as described above, because that does not have this limitation.
{% endhint %}

In order to configure SpecSync to generate the wrapper methods, the following steps have to be done in addition to the configuration steps described in [Step 0](synchronizing-automated-test-cases.md#suitebasedexecution-step0) of the [Test Suite based execution strategy](synchronizing-automated-test-cases.md#test-suite-based-execution-strategy).

### Step 0.1: Configure strategy

Specify `testSuiteBasedExecutionWithScenarioOutlineWrappers` as `testExecutionStrategy`in the [`synchronization/automation`](../configuration/configuration-synchronization/configuration-synchronization-automation.md) section of the configuration file.

```javascript
{
  ...
  "synchronization": {
    ...
    "automation": {
      "enabled": true,
      "testExecutionStrategy": "testSuiteBasedExecutionWithScenarioOutlineWrappers"
    },
    ...
  },
  ...
}
```

### Step 0.2: Install SpecSync SpecFlow plugin

Install the SpecSync SpecFlow plugin to your project as a NuGet package. For example for SpecFlow `v2.4.*`, install [`SpecSync.AzureDevOps.SpecFlow.2-4`](https://www.nuget.org/packages/SpecSync.AzureDevOps.SpecFlow.2-4).

```text
PM> Install-Package SpecSync.AzureDevOps.SpecFlow.2-4
```

{% hint style="warning" %}
If you use **SpecFlow v2** and the feature file code-behind files are not generated using the SpecFlow.Tools.MsBuild.Generation NuGet package, you need to force re-generating the code-behind files after installing the SpecSync.AzureDevOps.SpecFlow package. This can be done by invoking "Regenerate Feature Files" from the context menu of the SpecFlow project node in the "Solution Explorer" window in Visual Studio.
{% endhint %}

### Filter out wrapper methods in local test execution

For each Scenario Outline, there will be an additional wrapper test generated that will be used by the automated test case. Running these tests locally is unnecessary, therefore it is recommended to filter them out from local execution. This can be done for example by entering the `-Trait:SpecSyncWrapper` filter criteria to the search box of the "Test Explorer" window. \(See more in [SpecFlow configuration](../configuration/configuration-specflow.md).\) 

![Filtering out wrapper tests from local execution](../.gitbook/assets/getting-started-specflow-filter-out-wrapper.png)

### Filter out wrapper methods in Azure DevOps pipeline build

When the tests are run in Azure DevOps build from assembly \(so not through the test cases\), the generated wrapper methods have the be filtered out as well. This can be achieved by entering the `TestCategory!=SpecSyncWrapper` expression as "Test Filter criteria".

![Filter out scenario outline wrapper test methods in Azure DevOps build](../.gitbook/assets/automation-filter-wrapper-in-build.png)

## Use TestCase Data for Scenario Outline examples for legacy MsTest V1 projects

{% hint style="warning" %}
Note: this option is only available for legacy MsTest V1 based projects that use SpecFlow v2.3 or v2.4.

The legacy MsTest \(V1\) framework can be configured by adding a reference to the "Microsoft.VisualStudio.QualityTools.UnitTestFramework" assembly to your project. The MsTest-related NuGet packages \(MSTest.TestFramework, MSTest.TestAdapter\) cannot be used!
{% endhint %}

{% hint style="info" %}
This option was the default for SpecSync v1.\*.
{% endhint %}

The "Use TestCase Data" mode connects to the TFS TestCase during execution of Scenario Outlines to retrieve the examples. In this setup, the generated Scenario Outline Wrapper is not executable locally.

In order to configure SpecSync to use this option, you need to do the following steps:

1. Make sure your project uses MsTest V1 \(has a reference to the "Microsoft.VisualStudio.QualityTools.UnitTestFramework" assembly and does not use MsTest through NuGet\). Using this option with MsTest V2 will lead to an error, like _"The unit test adapter failed to connect to the data source or to read the data. \[...\] Unable to find the requested .Net Framework Data Provider. It may not be installed."_
2. Make sure your project uses SpecFlow v2.3 or v2.4
3. Setup SpecSync and the SpecSync SpecFlow plugin according to the [Test Suite based execution with Scenario Outline wrappers strategy](synchronizing-automated-test-cases.md#test-suite-based-execution-with-scenario-outline-wrappers-strategy) section.
4. Set specflow/scenarioOutlineAutomationWrappers in the configuration file to useTestCaseData.
5. Add a Test Settings file \(.testsettings\) to your project \(without any special configuration\) or a [Run Settings](https://docs.microsoft.com/en-us/visualstudio/test/configure-unit-tests-by-using-a-dot-runsettings-file?view=vs-2017#mstest-run-settings) file with `ForcedLegacyMode` set to `true`. You can download a sample Test Setting file from [here](https://www.specsolutions.eu/media/specsync/TestSuiteBasedRun.testsettings).
6. In the VsTest task of the build pipeline, browse the added Test or Run Settings file at the "Settings file" setting.

