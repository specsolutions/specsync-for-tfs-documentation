# publish-test-results

Publishes local test results to Azure DevOps server as a Test Run, where the results are connected to the synchronized test cases and optionally to a build.

To read more about publishing test results see the [Publishing test result files](../../features/test-result-publishing-features/publishing-test-result-files.md) page. Please also check the related [configuration file reference](../configuration/configuration-publishtestresults.md) for the `publish-test-results` command.

See more details about the command in the "Assembly based execution strategy" section of the [Synchronizing automated test cases](../../important-concepts/synchronizing-automated-test-cases.md) article.

## Options

In addition the the options listed here, all [common command line options](./#common-command-line-options) can also be used.

| Option | Description | Default |
| :--- | :--- | :--- |
| `--filter` | A [local test case condition](../../features/general-features/local-test-case-conditions.md) of scenarios that should be considered for test result publishing \(e.g. `@current_sprint and @done`\). See [Filters and scopes](../../important-concepts/filters-and-scopes.md) for details. | not filtered by tags |
| `--sourceFileFilter` | An expression of source file [glob patterns](https://speclink.me/specsync-glob) that should be considered for test result publishing (e.g. `Folder1/**/*.feature`). See [Filters and scopes](../../important-concepts/filters-and-scopes.md) for details. | not filtered by feature files |
| `-r`\|`--testResult` &lt;FILE&#x2011;PATH&gt; | The file path of the test result (.trx, .xml or .json) file to publish or a folder that contains multiple test result files. Can be specified multiple times. The semicolon-separated value list is still supported. Wildcards are supported using the [glob pattern](https://speclink.me/specsync-glob) syntax. | use from config file |
| `-f`\|`--testResultFormat` &lt;FORMAT&gt; | The file format of the file to publish. Please check the [Compatibility](../compatibility.md#supported-test-result-formats) page for supported formats. Invoking the command with `?` as format will list all supported format as well. | use from config file or detect automatically |
| `--runName` &lt;NAME&gt; | The name of the Test Run to be created. The value can contain [placeholders](../configuration/configuration-publishtestresults.md#setting-placeholders). | get from test result file |
| `--runComment` &lt;RUN&#x2011;COMMENT&gt; | The comment field of the test run to be created. The value can contain [placeholders](../configuration/configuration-publishtestresults.md#setting-placeholders). | not specified |
| `--testResultComment` &lt;RESULT&#x2011;COMMENT&gt; | The comment added to the individual test results within the created test run. Useful if the individual test results are typically browsed not through the test run. The value can contain [placeholders](../configuration/configuration-publishtestresults.md#setting-placeholders). | not specified |
| `--testRunSetting` &lt;NAME&gt;=&lt;VALUE&gt; | Additional settings for the created test run. The value can contain [placeholders](../configuration/configuration-publishtestresults.md#setting-placeholders). For Azure DevOps you can specify `name`, `comment`, `runType`, `buildId`, `buildNumber`, `buildFlavor`, `buildPlatform`. The build ID is a numeric ID of the build (e.g. `345`) and the build number is the configured build number (e.g. `20200119.1`). This option can be used *multiple times* using the `setting=value` format, e.g. `--testRunSetting "name=My Results"`. | use from config file or defaults |
| `--testResultSetting` &lt;NAME&gt;=&lt;VALUE&gt; | Additional settings for the individual test results. The value can contain [placeholders](../configuration/configuration-publishtestresults.md#setting-placeholders). For Azure DevOps test results you can specify `comment`. This option can be used *multiple times* using the `setting=value` format, e.g. `--testResultSetting "comment=Details"`. | use from config file or defaults |
| `--attachFile` &lt;FILE&gt; | File path that should be attached to the test run additionally. (e.g. `error1.png`) Can be specified multiple times. Wildcards are currently not supported. | only test result file attached |
| `-c`\|`--testConfiguration` &lt;CONFIGURATION&gt; | The Azure DevOps Test Configuration name or ID to publish the results for. For specifying an ID, use `#1234` format. | use from config file or detect automatically |
| `--testSuite` &lt;SUITE&#x2011;NAME&#x2011;OR&#x2011;ID&gt; | A Test Suite name or ID to publish the test results to. For specifying an ID, use `#1234` format. (e.g. `My Suite` or `#1234`) | use from config file |
| `--testPlan` &lt;PLAN&#x2011;NAME&#x2011;OR&#x2011;ID&gt; | The name or ID of the Test Plan to search the Test Suite in. For specifying an ID, use `#1234` format. (e.g. `My Plan` or `#345`) | all Test Plans are scanned through |
| `--disablePipelineAssociation` | If specified, the published test results will not be associated to the build or release pipeline. This is useful if the Azure DevOps project of the build is different to the project of the Test Cases. See [Troubleshooting entry](../../contact/troubleshooting.md#pipeline-not-found) for details. | pipelines are associated |

## Examples

Publishes a test result file `result.trx` to Azure DevOps:

```text
dotnet specsync publish-test-results --testResult result.trx
```

Publishes a test result file produced by Cucumber Java JUnit execution:

```text
dotnet specsync publish-test-results --testResult cucumber-result.xml --testResultFormat CucumberJavaJUnitXml
```

Publishes a test result file `result.trx` to Azure DevOps to the configured Test Suite for the Test Configuration `Windows 10`:

```text
dotnet specsync publish-test-results --testResult result.trx --testConfiguration "Windows 10"
```

Publishes test results to a specific Test Suite, where the Test Cases related to the executed scenarios are included. Test Plan ID is also specified for better performance.

```text
dotnet specsync publish-test-results --testPlan "My Plan" --testSuite "Ordering Tests" --testResult result.trx --testConfiguration "Windows 10"
```

{% page-ref page="./" %}

{% page-ref page="../configuration/configuration-publishtestresults.md" %}

