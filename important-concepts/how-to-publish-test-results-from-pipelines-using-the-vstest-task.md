# How to publish test results from pipelines using the VSTest task

The Azure DevOps pipelines provide two ways for executing tests in the pipeline: using the [.NET Core Task (`DotNetCoreCLI@2`)](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/dotnet-core-cli-v2?view=azure-pipelines) and the [Visual Studio Test Task (`VSTest@2`)](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/vstest-v2?view=azure-pipelines). SpecSync can publish the test results from both, but the settings are slightly different. The [How to use SpecSync from build or release pipeline](synchronizing-test-cases-from-build.md) guide shows the configuration options for the *.NET Core Task*, and in this guide you can find the configuration options required for the *Visual Studio Test Task* (referred as *VSTest* task in this guide).

Although the VSTest task is considered as a legacy execution options, there are still some features with this task that are not available with the *.NET Core Task*, most importantly the ability to [rerun failed tests](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/vstest-v2?view=azure-pipelines).

## Avoid duplicated test result reporting

When SpecSync publishes the test results from a pipeline, it associates them with the pipeline execution. This enables traceability (you can check which pipeline execution caused the failure) and also allows you to see the test results in the "Tests" tab of the pipeline result.

The VSTest task also publishes the test results and associates them to the build, although it cannot connect these results to the synchronized Test Cases. As a result you will see every test result twice in the "Tests" tab of the pipeline result. 

![](<../img/vstest-duplicated-test-results.png>)

Unfortunately the VSTest task does not provide an option to disable test result publishing (the *.NET Core Task* does have such option). So if the duplicated test result reporting causes a problem for you, you can choose from the following options:

1. Use the *.NET Core Task* instead to run the tests. See [How to use SpecSync from build or release pipeline](synchronizing-test-cases-from-build.md) guide for details.
2. Recommended: Instead of the VSTest task, use the identical [Visual Studio Test for SpecSync Task (`VsTestForSpecSync`)](how-to-use-the-pipeline-tasks.md). This task is based on the VSTest task and has been extended with an option to disable test result publishing. In order to use this task, you need to install the [*SpecSync Tools* Azure DevOps Extension](https://marketplace.visualstudio.com/items?itemName=SpecSolutions.specsync-tools&targetId=3063ff79-1c38-4e95-ba9c-795b1d1eb32e). You can find more information about the extension in the [How to use the SpecSync Azure DevOps pipeline tasks](how-to-use-the-pipeline-tasks.md) guide.
3. Disable build pipeline association for the test results published by SpecSync by specifying an additional `--disablePipelineAssociation` option. Please note that if you choose this option, the pipeline execution cannot be tracked from the published test results. You can add the build number as a comment for the test results though using the `--testResultComment` option.

{% hint style="info" %}
For the rest of this guide we will use the VsTestForSpecSync task in the examples, but the examples would be the same with the VSTest task as well, except the duplicated test result reporting.
{% endhint %}

The remaining sections of this guide describe the required configuration for the following situation:

* [Test result publishing with the VSTest / VsTestForSpecSync "rerun failed tests" option](#vstest-rerun)
* [Test result publishing with the VSTest / VsTestForSpecSync without "rerun failed tests" option](#vstest-single-result)
* [Test result publishing with the VSTest / VsTestForSpecSync "run tests in parallel by using multiple agents" option](#vstest-multiagent)

## Configuring test result publishing with the VSTest / VsTestForSpecSync "rerun failed tests" option <a href="vstest-rerun" id="vstest-rerun"></a>

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
* Test run and test result settings (e.g. `--runName`)
* Task conditions (_Run this task_ property)

To check the configuration options for these, please refer to the general [pipeline configuration guide](synchronizing-test-cases-from-build.md#publish-test-results-step2).

### Step 4: Configure the custom test result folder as test result file input

The test result files generated by the VSTest task have unpredictable names, so you cannot specify them explicitly using the `--testResultFile` option. To be able to load all TRX files from the test result folder, you need to specify the folder name instead. SpecSync will load and merge all TRX files from that folder and also recognize the test reruns. 

![](<../img/vstest-specify-test-result-folder-as-input-for-specsync.png>)

The following example shows the YAML view of a configured SpecSync publish-test-results task.

```bash
steps:
- task: DotNetCoreCLI@2
  displayName: 'specsync publish-test-results'
  inputs:
    command: custom
    custom: specsync
    arguments: 'publish-test-results --user "$(System.AccessToken)" --testResultFile $(Agent.TempDirectory)\TestResults\SpecFlowTests --runName "SpecFlow Tests"'
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

## Configuring test result publishing with the VSTest / VsTestForSpecSync "run tests in parallel by using multiple agents" option <a href="vstest-multiagent" id="vstest-multiagent"></a>

The VSTest task can be configured to run tests on multiple test agents parallel, as described in the ["Run tests in parallel using the Visual Studio Test task" Azure DevOps documentation page](https://learn.microsoft.com/en-us/azure/devops/pipelines/test/parallel-testing-vstest?view=azure-devops). Please note that this option is different from the in-process parallel execution options that many test frameworks (MsTest, NUnit, etc) provide and can be used with VSTest.

In order to follow the configuration steps required for SpecSync, please first get familiar with the concept of *running tests in parallel by using multiple agents* by following the Azure DevOps documentation page linked above. 

{% hint style="info" %}
When tests run in parallel by using multiple agents, each agent would run a set of tests and produce a TRX file for the results accordingly. In order to publish the aggregated test results, you will need to collect these individual TRX files and publish them once with SpecSync.

Unfortunately when VSTest runs on an agent, it deletes the TRX file after publishing it to its own Test Run, so you have to apply a workaround to get back the TRX file and make it available for SpecSync.
{% endhint %}

In order to run the tests with VSTest / VsTestForSpecSync task you need to configure three jobs for your pipeline. You will find the detailed setup instructions for them in the sections below.

1. The *main job* that typically builds your code and publishes the compiled test and production code as a Pipeline Artifact. In this description we refer to this as `drop` artifact.
2. The *agent job* that is configured to run "Multi-agent" with the specified number of agents. This job depends on the main job, so the agents run only after the main job has finished. This job contains the VSTest / VsTestForSpecSync task, that runs a set of the tests. We need to apply a workaround to get the TRX file generated and publish this TRX file as another Pipeline Artifact (called `testResults` in this description).
3. The *SpecSync job* that will download the `testResults` artifact with the TRX files and publishes them using SpecSync. This job might also contain a SpecSync synchronization as well ("push" command).

{% hint style="warning" %}
The current version of the VsTestForSpecSync task cannot hide the executed tests from the build result "Tests" tab, therefore they will be displayed duplicated. This limitation will be resolved in future versions of the task.
{% endhint %}


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
* should contain a VSTest / VsTestForSpecSync task, that runs a set of the tests

The following example shows the core configuration of the job with the first two tasks that downloads the artifacts from the main job and runs the tests from the downloaded artifacts folder.

```
dependsOn: Main_Job
pool:
  name: Azure Pipelines
  demands: vstest

steps:
- task: DownloadBuildArtifacts@1
  displayName: 'Download Build Artifacts'
  inputs:
    artifactName: drop

- task: SpecSolutions.specsync-tools.VSTestForSpecSync.VSTestForSpecSync@1
  displayName: 'VsTestForSpecSync - testAssemblies'
  inputs:
    testAssemblyVer2: |
     **\*test*.dll
     !**\*TestAdapter.dll
     !**\obj\**
    searchFolder: '$(System.ArtifactsDirectory)'
    testRunTitle: 'VsTestForSpecSync-Agents'
```

In order to publish the test results with SpecSync, you need to add two additional tasks after the VSTest / VsTestForSpecSync task.

A *PowerShell* task that executes a script that re-downloads the TRX file that the VSTest task has deleted. You can either download the script from the Spec Solutions website, include it to your repository and invoke it with the task or inline the script below. 

```PowerShell
$outputFolder = "TestResults"
$testRunId = $env:VSTEST_TESTRUNID
$pat = $env:SYSTEM_ACCESSTOKEN
$projectUrl = $env:SYSTEM_COLLECTIONURI + $env:SYSTEM_TEAMPROJECT

if ($testRunId -eq $null -or $testRunId -eq '') {
    Write-Error 'Test Run ID is not specified!' -ErrorAction Stop
}
if ($pat -eq $null -or $pat -eq '') {
    Write-Error 'PAT is not specified!' -ErrorAction Stop
}
if ($projectUrl -eq $null -or $projectUrl -eq '') {
    Write-Error 'Project URL is not specified!' -ErrorAction Stop
}
if ($outputFolder -eq $null -or $outputFolder -eq '') {
    Write-Error 'Output folder is not specified!' -ErrorAction Stop
}

Write-Host "Downloading attachments for Test Run #$testRunId in project $projectUrl"

$currentFolder = Get-Location
$fullOutputFolder = [System.IO.Path]::GetFullPath([System.IO.Path]::Combine($currentFolder, $outputFolder))
$_ = [System.IO.Directory]::CreateDirectory($fullOutputFolder)

$token = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes(":$($pat)"))
$header = @{Authorization = "Basic $token"}

$attachmentsResult = Invoke-RestMethod -Uri ("$projectUrl/_apis/test/runs/" + $testRunId + "/attachments?api-version=5.1") -Headers $header -Method Get

Write-Debug "attachments = $($attachmentsResult | ConvertTo-Json -Depth 100)"

$attachmentsResult.value | ForEach-Object {
    $attachmentUrl = $_.url
    $attachmentFileName = $_.fileName
    $outputFile = [System.IO.Path]::Combine($fullOutputFolder, $attachmentFileName)
    Write-Host "Downloading attachment $attachmentFileName"
    Write-Host "  from $attachmentUrl"
    Write-Host "  to $outputFile"
    Invoke-RestMethod -Uri ($attachmentUrl + "?api-version=5.1") -Headers $header -Method Get -OutFile $outputFile
}
```

This scrips gets the ID of the published Test Run from the `VSTEST_TESTRUNID` environment variable and downloads all Test Run attachments of it to the `TestResults` folder of the current folder. We recommend to set the working directory of the task to `$(Agent.TempDirectory)` so that the downloaded files will be in `$(Agent.TempDirectory)\TestResults`.

It is important to set the execution condition of this task in a way that it runs even if the previous tasks have failed, because we would like to preserve the TRX file even if the tests failed.

{% hint style="info" %}
The provided script downloads all TRX files from the Test Run, but since all agents will use the same Test Run, the same TRX files might be downloaded by multiple agents. This will not cause any functional problem, but might affect the performance in case of many agents and large TRX files. If this causes a problem, you can share the Test Run ID of the agent jobs (in environment variable `VSTEST_TESTRUNID`) with the SpecSync job and perform the TRX download there.
{% endhint %}

In order to access the Test Run the script uses the "job access token" that is described in the [Azure DevOps documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/access-tokens?view=azure-devops&tabs=yaml). To be able to use the Job access token in classic pipelines, you need to enable it for the job, by selecting the agent job and in the *Additional options* section, enable the setting *Allow scripts to access the OAuth token*. For YAML pipelines, the `SYSTEM_ACCESSTOKEN` environment variable needs to be declared, like in the example below. If you cannot use job access tokens, you should modify the script and set the `$pat` variable to a personal access token.

The following example shows how the task can be configured if the PowerShell script is executed from a file.

```
- task: PowerShell@2
  displayName: 'PowerShell Script'
  inputs:
    targetType: filePath
    filePath: './download-trx-from-testrun.ps1'
    workingDirectory: '$(Agent.TempDirectory)'
  env:
    SYSTEM_ACCESSTOKEN: $(System.AccessToken)    
  condition: succeededOrFailed()
```

Once the TRX files are downloaded, they have to be published, so that the SpecSync job will can access them. This can be done with a publish build artifacts task, like below. Again, make sure that the task is running even if the previous tasks have failed.

```
- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: testResults'
  inputs:
    PathtoPublish: '$(Agent.TempDirectory)\TestResults'
    ArtifactName: testResults
  condition: succeededOrFailed()
```

This completes the configuration of the Agent job. As a reference here is the fill YAML example that was shown above.

```
dependsOn: Main_Job
pool:
  name: Azure Pipelines
  demands: vstest

steps:
- task: DownloadBuildArtifacts@1
  displayName: 'Download Build Artifacts'
  inputs:
    artifactName: drop

- task: SpecSolutions.specsync-tools.VSTestForSpecSync.VSTestForSpecSync@1
  displayName: 'VsTestForSpecSync - testAssemblies'
  inputs:
    testAssemblyVer2: |
     **\*test*.dll
     !**\*TestAdapter.dll
     !**\obj\**
    searchFolder: '$(System.ArtifactsDirectory)'
    testRunTitle: 'VsTestForSpecSync-Agents'

- task: PowerShell@2
  displayName: 'PowerShell Script'
  inputs:
    targetType: filePath
    filePath: './download-trx-from-testrun.ps1'
    workingDirectory: '$(Agent.TempDirectory)'
  env:
    SYSTEM_ACCESSTOKEN: $(System.AccessToken)
  condition: succeededOrFailed()

- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: testResults'
  inputs:
    PathtoPublish: '$(Agent.TempDirectory)\TestResults'
    ArtifactName: testResults
  condition: succeededOrFailed()
```

### Setting up the SpecSync job

The configuration of the SpecSync job is similar to the genetic build pipeline configuration that is described in the [How to use SpecSync from build or release pipeline guide](synchronizing-test-cases-from-build.md), except that the test result TRX files are downloaded from the build artifact instead of created by a test execution task.

So in general the SpecSync job

* should be dependent on the Agent job (so that it only starts when all agents have finished),
* should run even if the previous jobs failed,
* should download the `testResult` artifact that was published by the agents,
* optionally should do a SpecSync "push" command to synchronize the scenarios (this can also be done in the main job),
* should publish the TRX files, by specifying the folder of these files instead of specifying a single TRX file path,
* might need to perform a "dotnet tool restore" command to download SpecSync to the job, if SpecSync is installed as a .NET Tool.

The following example shows how the job can be configured. 

```
dependsOn: Agent_Job
condition: succeededOrFailed()
pool:
  name: Azure Pipelines

steps:
- task: DownloadBuildArtifacts@1
  displayName: 'Download Build Artifacts'
  inputs:
    artifactName: testResults

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
    arguments: 'push --user "$(System.AccessToken)"'
    workingDirectory: MyCalculator.Specs

- task: DotNetCoreCLI@2
  displayName: 'specsync publish-test-results'
  inputs:
    command: custom
    custom: specsync
    arguments: 'publish-test-results --user "$(System.AccessToken)" -r $(System.ArtifactsDirectory)\testResults --runName "BDD Tests by SpecSync"'
    workingDirectory: MyCalculator.Specs
```
