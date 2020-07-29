# publish-test-results

Publishes local test results to Azure DevOps server as a Test Run, where the results are connected to the synchronized test cases and optionally to a build.

See more details about the command in the "Assembly based execution strategy" section of the [Synchronizing automated test cases](../../important-concepts/synchronizing-automated-test-cases.md) article.

## Options

In addition the the options listed here, all [common command line options](./#common-command-line-options) can also be used.

| Option | Description | Default |
| :--- | :--- | :--- |
| `--testConfiguration` &lt;CONFIGURATION&gt; | The Azure DevOps Test Configuration name or ID to publish the results for. For specifying an ID, use `#1234` format. | use from config file |
| `--testResultFile` &lt;FILE‑PATH&gt; | The file path of the test result \(.trx, .xml or .json\) file to publish. | use from config file |
| `--testResultFileFormat` &lt;FORMAT&gt; | The file format of the file to publish. Please check the [Compatibility](../compatibility.md#supported-test-result-formats) page for supported formats. Invoking the command with `?` as format will list all supported format as well. | use from config file or detect automatically |
| `--testSuite` | A Test Suite name or ID to publish the test results to. For specifying an ID, use `#1234` format. \(e.g. `My Suite` or `#1234`\) | use from config file |
| `--testPlanId` | The ID of the Test Plan to search the Test Suite in. \(e.g. `123`\) | all Test Plans are scanned through |
| `--runName` | The name of the Test Run to be created. | get from test result file |
| `--runComment` | The comment field of the Test Run to be created. | not specified |
| `--testResultComment` | The comment added to the individual test results within the created test run. Useful if the individual test results are typically browsed not through the Test Run. | not specified |
| `--buildId` | The build ID \(e.g. `345`\) of the build the test result was created for. | detect from current build |
| `--buildNumber` | The build number \(e.g. `20200119.1`\) of the build the test result was created for. Should be specified when build ID is not known. | detect from current build |
| `--buildFlavor` | The build flavor \(e.g. `Debug`\) of the build the test result was created for. Can only be specified if either `--buildNumber` or `--buildId` is specified. | detect from current build |
| `--buildPlatform` | he build flavor \(e.g. `x86`\) of the build the test result was created for. Can only be specified if either `--buildNumber` or `--buildId` is specified. | detect from current build |
| `--attachedFiles` | Semicolon separated list of file paths that should be attached to the test run additionally. \(e.g. `error1.png;error2.log`\) Wildcards are currently not supported. | only test result file attached |

## Examples

Publishes a test result file `result.trx` to Azure DevOps to the configured Test Suite for the Test Configuration `Windows 10`:

```text
dotnet specsync publish-test-result --testResultFile result.trx --testConfiguration "Windows 10"
```

Publishes a test result file produced by Cucumber Java JUnit execution:

```text
dotnet specsync publish-test-result --testResultFile cucumber-result.xml --testResultFileFormat CucumberJavaJUnitXml --testConfiguration "Windows 10"
```

Pushes test results to a specific Test Suite, where the Test Cases related to the executed scenarios are included. Test Plan ID is also specified for better performance.

```text
dotnet specsync publish-test-result --testPlanId 345 --testSuite "Ordering Tests" --testResultFile result.trx --testConfiguration "Windows 10"
```

{% page-ref page="./" %}

{% page-ref page="../configuration/publishtestresults.md" %}

