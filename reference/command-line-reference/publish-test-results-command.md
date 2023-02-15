# publish-test-results

Publishes local test results to Azure DevOps server as a Test Run, where the results are connected to the synchronized test cases and optionally to a build.

See more details about the command in the "Assembly based execution strategy" section of the [Synchronizing automated test cases](../../important-concepts/synchronizing-automated-test-cases.md) article.

## Options

In addition the the options listed here, all [common command line options](./#common-command-line-options) can also be used.

| Option | Description | Default |
| :--- | :--- | :--- |
| `--tagFilter` | A [local test case condition](../../features/general-features/local-test-case-conditions.md) of scenarios that should be considered for test result publishing \(e.g. `@current_sprint and @done`\). See [Filters and scopes](../../important-concepts/filters-and-scopes.md) for details. | not filtered by tags |
| `--sourceFileFilter` | An expression of source file [glob patterns](https://en.wikipedia.org/wiki/Glob_%28programming%29) that should be considered for test result publishing (e.g. `Folder1/**/*.feature`). See [Filters and scopes](../../important-concepts/filters-and-scopes.md) for details. | not filtered by feature files |
| `-r`\|`--testResultFile` &lt;FILE&#x2011;PATH&gt; | The file path of the test result (.trx, .xml or .json) file to publish or a folder that contains multiple test result files. Multiple paths can be listed, separated by semicolon (`;`). | use from config file |
| `-f`\|`--testResultFileFormat` &lt;FORMAT&gt; | The file format of the file to publish. Please check the [Compatibility](../compatibility.md#supported-test-result-formats) page for supported formats. Invoking the command with `?` as format will list all supported format as well. | use from config file or detect automatically |
| `--runName` &lt;NAME&gt; | The name of the Test Run to be created. The value can contain [placeholders](../configuration/configuration-publishtestresults.md#setting-placeholders). | get from test result file |
| `--attachedFiles` &lt;FILE&#x2011;LIST&gt; | Semicolon separated list of file paths that should be attached to the test run additionally. (e.g. `error1.png;error2.log`) Wildcards are currently not supported. | only test result file attached |
| `-c`\|`--testConfiguration` &lt;CONFIGURATION&gt; | The Azure DevOps Test Configuration name or ID to publish the results for. For specifying an ID, use `#1234` format. | use from config file or detect automatically |
| `--testSuite` &lt;SUITE&#x2011;NAME&#x2011;OR&#x2011;ID&gt; | A Test Suite name or ID to publish the test results to. For specifying an ID, use `#1234` format. (e.g. `My Suite` or `#1234`) | use from config file |
| `--testPlanId` &lt;PLAN&#x2011;ID&gt; | The ID of the Test Plan to search the Test Suite in. (e.g. `123`) | all Test Plans are scanned through |
| `--runComment` &lt;RUN&#x2011;COMMENT&gt; | The comment field of the Test Run to be created. The value can contain [placeholders](../configuration/configuration-publishtestresults.md#setting-placeholders). | not specified |
| `--testResultComment` &lt;RESULT&#x2011;COMMENT&gt; | The comment added to the individual test results within the created test run. Useful if the individual test results are typically browsed not through the Test Run. The value can contain [placeholders](../configuration/configuration-publishtestresults.md#setting-placeholders). | not specified |
| `--buildId` &lt;BUILD&#x2011;ID&gt; | The build ID (e.g. `345`) of the build the test result was created for. To prevent detecting build from build you can specify the `--disablePipelineAssociation` option (or set the `--buildId` option to an empty value before v3.3.3). | detect from current build |
| `--buildNumber` &lt;BUILD&#x2011;NUMBER&gt; | The build number (e.g. `20200119.1`) of the build the test result was created for. Should be specified when build ID is not known. | build ID is used |
| `--buildFlavor` &lt;FLAVOR&gt; | The build flavor (e.g. `Debug`) of the build the test result was created for. Can only be specified if either `--buildNumber` or `--buildId` is specified. | detect from current build |
| `--buildPlatform` &lt;PLATFORM&gt; | The build platform (e.g. `x86`) of the build the test result was created for. Can only be specified if either `--buildNumber` or `--buildId` is specified. | detect from current build |
| `--disablePipelineAssociation` | If specified, the published test results will not be associated to the build or release pipeline. This is useful if the Azure DevOps project of the build is different to the project of the Test Cases. See [Troubleshooting entry](../../contact/troubleshooting.md#pipeline-not-found) for details. | pipelines are associated |

## Examples

Publishes a test result file `result.trx` to Azure DevOps:

```text
dotnet specsync publish-test-results --testResultFile result.trx
```

Publishes a test result file produced by Cucumber Java JUnit execution:

```text
dotnet specsync publish-test-results --testResultFile cucumber-result.xml --testResultFileFormat CucumberJavaJUnitXml
```

Publishes a test result file `result.trx` to Azure DevOps to the configured Test Suite for the Test Configuration `Windows 10`:

```text
dotnet specsync publish-test-results --testResultFile result.trx --testConfiguration "Windows 10"
```

Publishes test results to a specific Test Suite, where the Test Cases related to the executed scenarios are included. Test Plan ID is also specified for better performance.

```text
dotnet specsync publish-test-results --testPlanId 345 --testSuite "Ordering Tests" --testResultFile result.trx --testConfiguration "Windows 10"
```

{% page-ref page="./" %}

{% page-ref page="../configuration/configuration-publishtestresults.md" %}

