# How to publish test results from pipelines using the VSTest task

The Azure DevOps pipelines provide two ways for executing tests in the pipeline: using the [.NET Core Task (`DotNetCoreCLI@2`)](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/dotnet-core-cli-v2?view=azure-pipelines) and the [Visual Studio Test Task (`VSTest@2`)](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/vstest-v2?view=azure-pipelines). SpecSync can publish the test results from both, but the settings are slightly different. The [How to use SpecSync from build or release pipeline](synchronizing-test-cases-from-build.md) guide shows the configuration options for the *.NET Core Task*, and in this guide you can find the configuration options required for the *Visual Studio Test Task* (referred as *VSTest* task in this guide).

Although the VSTest task is considered as a legacy execution options, there are still some features with this task that are not available with the *.NET Core Task*, most importantly the ability to [rerun failed tests](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/vstest-v2?view=azure-pipelines) and the execution of tests [distributed to multiple agents](https://learn.microsoft.com/en-us/azure/devops/pipelines/test/parallel-testing-vstest?view=azure-devops).

## Avoid duplicated test result reporting

When SpecSync publishes the test results from a pipeline, it associates them with the pipeline execution. This enables traceability (you can check which pipeline execution caused the failure) and also allows you to see the test results in the "Tests" tab of the pipeline result.

The VSTest task also publishes the test results and associates them to the build, although it cannot connect these results to the synchronized Test Cases. As a result you will see every test result twice in the "Tests" tab of the pipeline result. 

![](<../img/vstest-duplicated-test-results.png>)

Unfortunately the VSTest task does not provide an option to disable test result publishing (the *.NET Core Task* does have such option). So if the duplicated test result reporting causes a problem for you, you can choose from the following options:

1. Use the *.NET Core Task* instead to run the tests. See [How to use SpecSync from build or release pipeline](synchronizing-test-cases-from-build.md) guide for details.
2. Instead of the VSTest task, use the identical [Visual Studio Test for SpecSync Task (`VsTestForSpecSync`)](how-to-use-the-pipeline-tasks.md). This task is based on the VSTest task and has been extended with an option to disable test result publishing. In order to use this task, you need to install the [*SpecSync Tools* Azure DevOps Extension](https://marketplace.visualstudio.com/items?itemName=SpecSolutions.specsync-tools&targetId=3063ff79-1c38-4e95-ba9c-795b1d1eb32e). You can find more information about the extension in the [How to use the SpecSync Azure DevOps pipeline tasks](how-to-use-the-pipeline-tasks.md) guide. *This method cannot be used to avoid duplicated test result reporting when the tests are running on multiple agents or when you use the "specify a batch size" option of VSTest.*
3. Use the 'testRunTrx' test result loader with SpecSync that collects the test result details from the Test Run published by the VSTest task and re-publishes them with the connection to the Test Cases. The loader will set the status of the original Test Run to `Not Started`, so their results are not shown in the "Tests" tab of the pipeline result. See the [Test result publishing with the VSTest "run tests in parallel by using multiple agents" option](#vstest-multiagent) and [Test result publishing with the VSTest "specify a batch size" option](#vstest-batchsize) sections below for details about this option.
4. Disable build pipeline association for the test results published by SpecSync by specifying an additional `--disablePipelineAssociation` option. Please note that if you choose this option, the pipeline execution cannot be tracked from the published test results. You can add the build number as a comment for the test results though using the `--testResultSetting comment="..."` option.

{% hint style="info" %}
For the rest of this guide we will use the VsTestForSpecSync task in the examples, but the examples would be the same with the VSTest task as well, except the duplicated test result reporting.
{% endhint %}

The remaining sections of this guide describe the required configuration for the following situation:

* [Test result publishing with the VSTest / VsTestForSpecSync "rerun failed tests" option](#vstest-rerun)
* [Test result publishing with the VSTest / VsTestForSpecSync without "rerun failed tests" option](#vstest-single-result)
* [Test result publishing with the VSTest "run tests in parallel by using multiple agents" option](#vstest-multiagent)
* [Test result publishing with the VSTest "specify a batch size" option](#vstest-batchsize)

## Configuring test result publishing with the VSTest / VsTestForSpecSync "rerun failed tests" option <a href="vstest-rerun" id="vstest-rerun"></a>

{% hint style="info" %}
The rerun option of the recent versions of the VSTest / VsTestForSpecSync task is not compatible with xUnit and NUnit data-driven tests that are used by SpecFlow for scenario outlines. To overcome this issue, there is a workaround available by setting the "batch size" to a large enough number. This workaround is not compatible with the solution described in this section, but you should use the solution described at [Test result publishing with the VSTest "specify a batch size" option](#vstest-batchsize).
{% endhint %}

{% hint style="info" %}
When the "rerun failed tests" option is enabled for the VSTest task, it may run the tests multiple times and generate multiple test result (TRX) files. Because SpecSync uses the test result files to process and publish the results the configuration of the tasks are different for this case. If you don't use and don't plan to use the "rerun failed tests" option, you can also configure SpecSync with a single test result file as [described below](#vstest-single-result). It is recommended to use the configuration described in this section, to reduce the configuration efforts in case you would like to enable the "rerun failed tests" option later.
{% endhint %}

In order to configure the test result publishing with the VSTest / VsTestForSpecSync task, you need to perform a few simple configuration steps. Let's start with the configuration settings required for the VSTest / VsTestForSpecSync task.

### Step 1: Do not specify output file name

When "rerun failed tests" option is enabled for the VSTest task, it may generate multiple TRX test result file. With this feature enabled, you *cannot* specify the exact test result file name (with the `/logger:trx;LogFileName=my-results.trx` option or with the "Test result file" option of the VsTestForSpecSync task), because then the test result files generated during the rerun would override the main test result file.

So to be able track all results, **the test result file name must not be specified**. The task fill generate unique file names with the timestamp of the execution of each rerun.

### Step 2: Specify a custom output folder

By default the VSTest task saves the test result files to the `$(Agent.TempDirectory)\TestResults` folder. In order to separate the test result files of this task from the test results of the other VSTest tasks in the pipeline, it is recommended to specify a custom output folder for the VSTest task. For example you can add the test project name to the path: `$(Agent.TempDirectory)\TestResults\MyTestProject`. The folder will be created automatically by VSTest.

![](<../img/vstest-specify-test-result-folder.png>)

The following example shows the YAML view of a configured VsTestForSpecSync task.

```bash
steps:
- task: SpecSolutions.specsync-tools.VSTestForSpecSync.VSTestForSpecSync@1
  displayName: 'VsTestForSpecSync - specflow tests'
  inputs:
    testAssemblyVer2: |
     **\*SpecFlowTests.dll
     !**\*TestAdapter.dll
     !**\obj\**
    resultsFolder: '$(Agent.TempDirectory)\TestResults\SpecFlowTests'
    rerunFailedTests: true
```

Following the general pipeline setup concept described in the [How to use SpecSync from build or release pipeline](synchronizing-test-cases-from-build.md) guide, the next step should be a SpecSync `publish-test-results` step, for example using the *.NET Core Task*. For this step the following configuration options should be applied.

### Step 3: Configure usual publish-test-results configuration options

The SpecSync `publish-test-results` command provides a couple of configuration options that are independent of whether you run the tests with the VSTest task or using and other task execution option. These settings are:

* Working Directory
* Authentication (`--user`)
* Test Configuration (`--testConfiguration`)
* Test run and test result settings (e.g. `--runName "My Results"` or `--testRunSetting "name=My Results"`)
* Task conditions (_Run this task_ property)

To check the configuration options for these, please refer to the general [pipeline configuration guide](synchronizing-test-cases-from-build.md#publish-test-results-step2).

### Step 4: Configure the custom test result folder as test result file input

The test result files generated by the VSTest task have unpredictable names, so you cannot specify them explicitly using the `--testResult` option. To be able to load all TRX files from the test result folder, you need to specify the folder name instead. SpecSync will load and merge all TRX files from that folder and also recognize the test reruns. 

![](<../img/vstest-specify-test-result-folder-as-input-for-specsync.png>)

The following example shows the YAML view of a configured SpecSync publish-test-results task.

```bash
steps:
- task: DotNetCoreCLI@2
  displayName: 'specsync publish-test-results'
  inputs:
    command: custom
    custom: specsync
    arguments: 'publish-test-results --user "$(System.AccessToken)" --testResult $(Agent.TempDirectory)\TestResults\SpecFlowTests --runName "SpecFlow Tests"'
    workingDirectory: PipelineDemo.SpecFlowTests
  condition: and(succeededOrFailed(), ne(variables['Build.Reason'], 'PullRequest'))
  ```

### Step 5: Verify configuration

Run the pipeline. If any of your tests fail for the first run, the VSTest task will rerun the failed once. The result should be visible in the "Tests" tab of your pipeline execution. Please note that if a test pass during a rerun, the VSTask will consider that as a "passing" test, so it will not be visible by default in the "Tests" tab. In order to see all test results, clear the filters first.

![](<../img/vstest-rerun-results.png>)

## Configuring test result publishing with the VSTest / VsTestForSpecSync without "rerun failed tests" option <a href="vstest-single-result" id="vstest-single-result"></a>

If you don't plan to use the "rerun failed tests" option option of the VSTest task, you can also configure the test result publishing with a single test result file. For that the following configuration steps have to be performed.

### Step 1: Specify output file name

The VSTest task does not have a direct option to specify the TRX test result file name, but you can set that by specifying `/logger:trx;LogFileName=my-results.trx` in the "Other console options" property. 

![](<../img/vstest-single-test-result-file.png>)

If you use the VsTestForSpecSync task, you can also use the "Test result file" setting in the "SpecSync options" group. 

![](<../img/vstest-single-test-result-file-VSTestForSpecSync.png>)

The following example shows the YAML view of a configured VsTestForSpecSync task.

```bash
steps:
- task: SpecSolutions.specsync-tools.VSTestForSpecSync.VSTestForSpecSync@1
  displayName: 'VsTestForSpecSync - specflow tests'
  inputs:
    testAssemblyVer2: |
     **\*SpecFlowTests.dll
     !**\*TestAdapter.dll
     !**\obj\**
    testResultFile: 'specflow-test-results.trx'
```

### Step 2: Configure publish-test-results configuration options

Configure the SpecSync publish-test-results task according to the [pipeline configuration guide](synchronizing-test-cases-from-build.md#publish-test-results-step2).

### Step 3: Verify configuration

Run the pipeline. If you have used the VsTestForSpecSync task, the results will be displayed only once.

![](<../img/vstest-results.png>)

## Configuring test result publishing with the VSTest "run tests in parallel by using multiple agents" option <a href="vstest-multiagent" id="vstest-multiagent"></a>

{% hint style="info" %}
The VsTestForSpecSync task cannot hide the test results from the "Tests" tab of the pipeline result when the tests are running on multiple agents. Therefore for this option the normal VSTest task should be used, although the VsTestForSpecSync works as well.
{% endhint %}

The VSTest task can be configured to run tests on multiple test agents parallel, as described in the ["Run tests in parallel using the Visual Studio Test task" Azure DevOps documentation page](https://learn.microsoft.com/en-us/azure/devops/pipelines/test/parallel-testing-vstest?view=azure-devops). Please note that this option is different from the in-process parallel execution options that many test frameworks (MsTest, NUnit, etc) provide and can be used with VSTest.

In order to follow the configuration steps required for SpecSync, please first get familiar with the concept of *running tests in parallel by using multiple agents* by following the Azure DevOps documentation page linked above. 

{% hint style="info" %}
When tests run in parallel by using multiple agents, each agent runs a set of tests and produce a TRX file for the results accordingly and publishes them to a shared Test Run. 

Unfortunately when VSTest runs on an agent, it deletes the TRX file after publishing it to its own Test Run, so you cannot collect the TRX files from the agents and publish them with SpecSync as usual.

This guide shows how you can still achieve the expected result with the `testRunTrx` test result loader of SpecSync introduced in v3.4.8.
{% endhint %}

In order to run the tests with VSTest task you need to configure three jobs for your pipeline. You will find the detailed setup instructions for them in the sections below.

1. The *main job* that typically builds your code and publishes the compiled test and production code as a Pipeline Artifact. In this description we refer to this as `drop` artifact.
2. The *agent job* that is configured to run "Multi-agent" with the specified number of agents. This job depends on the main job, so the agents run only after the main job has finished. This job contains the VSTest task, that runs a set of the tests. We need to collect the ID of Test Run created by the agents (all agent uses the same Test Run) to be able to share it with the *SpecSync job*.
3. The *SpecSync job* that will get the ID of the Test Run published by the agents and re-publishes the test results from it using SpecSync. This job might also contain a SpecSync synchronization as well ("push" command). This job depends on the agent job, so it runs when all agent jobs have finished.


### Setting up the main job

The main job should be configured according to the Azure DevOps documentation linked above. There are no special consideration required because of SpecSync.

The following YAML snippet shows an example of a simple main job that simply builds the C# projects and publishes them as `drop` artifact.

```bash
pool:
  name: Azure Pipelines

steps:
- task: DotNetCoreCLI@2
  displayName: Build
  inputs:
    projects: '**/*.csproj'
    arguments: '--configuration $(BuildConfiguration)'

- task: CopyFiles@2
  displayName: 'Copy Files to: $(build.artifactstagingdirectory)'
  inputs:
    SourceFolder: '$(system.defaultworkingdirectory)'
    Contents: '**\bin\$(BuildConfiguration)\**'
    TargetFolder: '$(build.artifactstagingdirectory)'
  condition: succeededOrFailed()

- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact'
  inputs:
    PathtoPublish: '$(build.artifactstagingdirectory)'
  condition: succeededOrFailed()
```

### Setting up the agent job

The configuration of the agent job should also mainly follow the guides in the Azure DevOps documentation linked above:

* should be set as "Multi-agent"
* should be dependent on the main job
* should download the artifacts from the main job
* should contain a VSTest task, that runs a set of the tests

The following example shows the core configuration of the job with the first two tasks that downloads the artifacts from the main job and runs the tests from the downloaded artifacts folder.

```
dependsOn: Main_Job
pool:
  name: Azure Pipelines
  demands: vstest
strategy:
  parallel: 2

steps:
- task: DownloadBuildArtifacts@1
  displayName: 'Download Build Artifacts'
  inputs:
    artifactName: drop

- task: VSTest@3
  displayName: 'VsTest - testAssemblies'
  inputs:
    testAssemblyVer2: |
     **\*test*.dll
     !**\*TestAdapter.dll
     !**\obj\**
    searchFolder: '$(System.ArtifactsDirectory)'
    testRunTitle: 'VsTest-Agents'
```

The ID of the created Test Run is saved to the `VSTEST_TESTRUNID` environment variable by the VSTest task. This is what we need to share with the *SpecSync job*. With YAML pipelines the ID can be simply shared using output variables as described in the [Azure DevOps documentation](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&tabs=yaml%2Cbatch#use-output-variables-from-tasks). Unfortunately in Classic pipelines this option is not available so a workaround needs to be used. 

In this document we show a simple workaround by sharing the variable via shared artifact file. This we can achieve using a *PowerShell* task like this:

```PowerShell
$env:VSTEST_TESTRUNID | Out-File -FilePath $(Agent.TempDirectory)\VSTestTestRunId.txt
Write-Host "##vso[artifact.upload containerfolder=VSTestTestRunId;artifactname=VSTestTestRunId]$(Agent.TempDirectory)\VSTestTestRunId.txt"
```

This script saves the content of the `VSTEST_TESTRUNID` environment variable to a file `VSTestTestRunId.txt` and then uploads it as a build artifact named `VSTestTestRunId`.

The following example shows the complete task invoking the script. It is configured to run even if the previous task failed, because we want to capture the results even if the tests fail.

```
- powershell: |
   $env:VSTEST_TESTRUNID | Out-File -FilePath $(Agent.TempDirectory)VSTestTestRunId.txt
   Write-Host "##vso[artifact.upload containerfolder=VSTestTestRunId;artifactname=VSTestTestRunId]$(Agent.TempDirectory)\VSTestTestRunId.txt"
  displayName: 'PowerShell Script - Save Test Run ID'
  condition: succeededOrFailed()
```

This completes the configuration of the Agent job. As a reference here is the fill YAML example that was shown above.

```
dependsOn: Main_Job
pool:
  name: Azure Pipelines
  demands: vstest
strategy:
  parallel: 2

steps:
- task: DownloadBuildArtifacts@1
  displayName: 'Download Build Artifacts'
  inputs:
    artifactName: drop

- task: VSTest@3
  displayName: 'VsTest - testAssemblies'
  inputs:
    testAssemblyVer2: |
     **\*test*.dll
     !**\*TestAdapter.dll
     !**\obj\**
    searchFolder: '$(System.ArtifactsDirectory)'
    testRunTitle: 'VsTest-Agents'

- powershell: |
   $env:VSTEST_TESTRUNID | Out-File -FilePath $(Agent.TempDirectory)VSTestTestRunId.txt
   Write-Host "##vso[artifact.upload containerfolder=VSTestTestRunId;artifactname=VSTestTestRunId]$(Agent.TempDirectory)\VSTestTestRunId.txt"
  displayName: 'PowerShell Script - Save Test Run ID'
  condition: succeededOrFailed()
```

### Setting up the SpecSync job

The configuration of the SpecSync job is similar to the genetic build pipeline configuration that is described in the [How to use SpecSync from build or release pipeline guide](synchronizing-test-cases-from-build.md), except that as an input not the TRX files should be provided but the ID of the Test Run received from the agent jobs.

So in general the SpecSync job

* should be dependent on the Agent job (so that it only starts when all agents have finished),
* should run even if the previous jobs failed,
* get the Test Run ID via output variables for YAML pipelines of through the `VSTestTestRunId` artifact that was published by the agents,
* optionally should do a SpecSync "push" command to synchronize the scenarios (this can also be done in the main job),
* should publish the results by specifying the Test Run ID with the `-r` option to SpecSync,
* might need to perform a "dotnet tool restore" command to download SpecSync to the job, if SpecSync is installed as a [.NET Tool](../installation/dotnet-core-tool.md).

For Classic pipelines, the Test Run ID can be received from the build artifact named `VSTestTestRunId` with the following PowerShell script. 

```PowerShell
$testRunIdFromAgent = get-content $(System.ArtifactsDirectory)\VSTestTestRunId\VSTestTestRunId.txt
Write-Host "##vso[task.setvariable variable=VSTestTestRunId;isOutput=true]$testRunIdFromAgent"
Write-Host "Received TestRun ID from Agent: $testRunIdFromAgent"
```

The script sets the ID to a variable `VSTestTestRunId`. To be able to use this variable in the subsequent tasks, the "Reference name" setting of the task within the "Output Variables" group must be set. In this example it was set to `GetVSTestTestRunId`, so the variable can be used later as `$(GetVSTestTestRunId.VSTestTestRunId)`.

When invoking the SpecSync publish-test-results command, we need to provide `-r $(GetVSTestTestRunId.VSTestTestRunId) -f TestRunTrx` in order to get the results from the Test Run.

The loader will set the status of the original Test Run to `Not Started`, so their results are not shown in the "Tests" tab of the pipeline result.

The following example shows how the entire job can be configured. 

```
dependsOn: Agent_Job
condition: succeededOrFailed()
pool:
  name: Azure Pipelines

steps:
- task: DownloadBuildArtifacts@1
  displayName: 'Download Test Run ID file'
  inputs:
    artifactName: VSTestTestRunId

- powershell: |
   $testRunIdFromAgent = get-content $(System.ArtifactsDirectory)\testResults\VSTestTestRunId.txt
   Write-Host "##vso[task.setvariable variable=VSTestTestRunId;isOutput=true]$testRunIdFromAgent"
   Write-Host "Received TestRun ID from Agent: $testRunIdFromAgent"
  name: GetVSTestTestRunId
  displayName: 'PowerShell Script - Get VSTest TestRun ID from Agent'

- task: DotNetCoreCLI@2
  displayName: 'dotnet tool restore'
  inputs:
    command: custom
    custom: tool
    arguments: restore

- task: DotNetCoreCLI@2
  displayName: 'specsync push'
  inputs:
    command: custom
    custom: specsync
    arguments: 'push --disableLocalChanges --user "$(System.AccessToken)"'
    workingDirectory: MyCalculator.Specs

- task: DotNetCoreCLI@2
  displayName: 'specsync publish-test-results'
  inputs:
    command: custom
    custom: specsync
    arguments: 'publish-test-results --user "$(System.AccessToken)" -r $(GetVSTestTestRunId.VSTestTestRunId) -f TestRunTrx --runName "BDD Tests by SpecSync"'
    workingDirectory: MyCalculator.Specs
```

## Test result publishing with the VSTest "specify a batch size" option <a href="vstest-batchsize" id="vstest-batchsize"></a>

{% hint style="info" %}
The VsTestForSpecSync task cannot hide the test results from the "Tests" tab of the pipeline result when the tests are running on multiple agents. Therefore for this option the normal VSTest task should be used, although the VsTestForSpecSync works as well.
{% endhint %}

The rerun option of the recent versions of the VSTest task is not compatible with xUnit and NUnit data-driven tests that are used by SpecFlow for scenario outlines. If you try to use rerun in such configuration, the failed tests will not be re-executed and an error is reported like "Error occurred while publishing test results : System.Exception: No tests were discovered while attempting to rerun failed tests. This is likely to happen if these tests are xunit or nunit datadriven/parameterized tests or tests with custom display names with spaces or special characters which are not supported with rerun."

A generally known workaround for this problem is to set the "Batch options" setting of the VSTest task to "Specify a batch size" and specify a large enough number as "Number of tests per batch", e.g. "100000". With this setting the VSTest task handles the rerun using a different method that works also for xUnit and NUnit as well.

The only downside of this setting is that VSTest deletes the generated TRX files and other attachments after the run so SpecSync cannot publish them using the usual way.

Fortunately the solution described in [Test result publishing with the VSTest "run tests in parallel by using multiple agents" option](#vstest-multiagent) above also works in this case: by using the new `testRunTrx` loader of SpecSync, you can configure SpecSync to publish the test results from the Test Run created by the VSTest task. 

In this section we describe the configuration settings required for this setup.

First of all, configure the VSTest task for rerun as usual, but select "Specify a batch size" as "Batch options" within the "Advanced execution options" section and set a large enough number as "Number of tests per batch", e.g. "100000".

![](<../img/vstest-specify-batch-size.png>)

An example task configuration might look like this in YAML.

```
- task: VSTest@3
  displayName: 'VsTestV3 - SpecFlow xUnit Tests'
  inputs:
    testAssemblyVer2: 'MyCalculator.Specs\bin\$(BuildConfiguration)\net6.0\MyCalculator.Specs.dll'
    batchingBasedOnAgentsOption: customBatchSize
    customBatchSizeValue: 100000
    testRunTitle: 'VsTestV3 - SpecFlow xUnit Tests'
    rerunFailedTests: true
    rerunFailedThreshold: 99
```

When the task will execute it will save the ID of the created Test Run to an environment variable `VSTEST_TESTRUNID` that can be processed by SpecSync if you use the `testRunTrx` test result folder by specifying `-f testRunTrx`.

An example of the SpecSync task might look like this.

```
steps:
- task: DotNetCoreCLI@2
  displayName: 'specsync publish-test-results'
  inputs:
    command: custom
    custom: specsync
    arguments: 'publish-test-results --user "$(System.AccessToken)" -f testRunTrx --runName "SpecFlow Tests"'
    workingDirectory: MyCalculator.Specs
  condition: succeededOrFailed()
```

With this settings SpecSync will load the results including the rerun results and publish them to the linked Test Cases.

The loader will set the status of the original Test Run to `Not Started`, so their results are not shown in the "Tests" tab of the pipeline result.

