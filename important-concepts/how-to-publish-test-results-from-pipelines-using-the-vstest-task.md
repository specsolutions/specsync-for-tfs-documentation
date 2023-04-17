# How to publish test results from pipelines using the VSTest task

The Azure DevOps pipelines provide two ways for executing tests in the pipeline: using the [.NET Core Task (`DotNetCoreCLI@2`)](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/dotnet-core-cli-v2?view=azure-pipelines) and the [Visual Studio Test Task (`VSTest@2`)](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/vstest-v2?view=azure-pipelines). SpecSync can publish the test results from both, but the settings are slightly different. The [How to use SpecSync from build or release pipeline](synchronizing-test-cases-from-build.md) guide shows the configuration options for the *.NET Core Task*, and in this guide you can find the configuration options required for the *Visual Studio Test Task* (referred as *VSTest* task in this guide).

Although the VSTest task is considered as a legacy execution options, there are still some features with this task that are not available with the *.NET Core Task*, most importantly the ability to [rerun failed tests](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/vstest-v2?view=azure-pipelines).

## Avoid duplicated test result reporting

When SpecSync publishes the test results from a pipeline, it associates them with the pipeline execution. This enables traceability (you can check which pipeline execution caused the failure) and also allows you to see the test results in the "Tests" tab of the pipeline result.

The VSTest task also publishes the test results and associates them to the build, although it cannot connect these results to the synchronized Test Cases. As a result you will see every test result twice in the "Tests" tab of the pipeline result. 

![](<../img/vstest-duplicated-test-results.png>)

Unfortunately the VSTest task does not provide an option to disable test result publishing (the *.NET Core Task* does have such option). So if the duplicated test result reporting causes a problem for you, you can choose from the following options:

1. Use the *.NET Core Task* instead to run the tests.
2. Recommended: Instead of the VSTest task, use the identical [Visual Studio Test for SpecSync Task (`VsTestForSpecSync`)](how-to-use-the-pipeline-tasks.md). This task is based on the VSTest task and has been extended with an option to disable test result publishing. In order to use this task, you need to install the [*SpecSync Tools* Azure DevOps Extension](https://marketplace.visualstudio.com/items?itemName=SpecSolutions.specsync-tools&targetId=3063ff79-1c38-4e95-ba9c-795b1d1eb32e). You can find more information about the extension in the [How to use the SpecSync Azure DevOps pipeline tasks](how-to-use-the-pipeline-tasks.md) guide.
3. Disable build pipeline association for the test results published by SpecSync by specifying an additional `--disablePipelineAssociation` option. Please note that if you choose this option, the pipeline execution cannot be tracked from the published test results. You can add the build number as a comment for the test results though using the `--testResultComment` option.

{% hint style="info" %}
For the rest of this guide we will use the VsTestForSpecSync task in the examples, but the examples would be the same with the VSTest task as well, except the duplicated test result reporting.
{% endhint %}

## Configuring test result publishing with the VSTest / VsTestForSpecSync "rerun failed tests" option <a href="vstest-rerun" id="vstest-rerun"></a>

{% hint style="info" %}
When the "rerun failed tests" option is enabled for the VSTest task, it may run the tests multiple times and generate multiple test result (TRX) files. Because SpecSync uses the test result files to process and publish the results the configuration of the tasks are different for this case. If you don't use and don't plan to use the "rerun failed tests" option, you can also configure SpecSync with a single test result file as [described below](#vstest-single-result). It is recommended to use the configuration described in this section, to reduce the configuration efforts in case you would like to enable the "rerun failed tests" option later.
{% endhint %}

In order to configure the test result publishing with the VSTest / VsTestForSpecSync task, you need to perform a few simple configuration steps. Let's start with the configuration settings required for the VSTest / VsTestForSpecSync task.

### Step 1: Do not specify output file name

When "rerun failed tests" option is enabled for the VSTest task, it may generate multiple TRX test result file. With this feature enabled, you **cannot** specify the exact test result file name (with the `/logger:trx;LogFileName=my-results.trx` option or with the "Test result file" option of the VsTestForSpecSync task), because then the test result files generated during the rerun would override the main test result file.

So to be able track all results, the test result file name **must not be specified**. The task fill generate unique file names with the timestamp of the execution of each rerun.

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
