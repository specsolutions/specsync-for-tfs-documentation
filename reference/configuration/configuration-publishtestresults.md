# publishTestResults

This configuration section contains settings related to publishing test results. 

To read more about publishing test results see the [Publishing test result files](../../features/test-result-publishing-features/publishing-test-result-files.md) page. Please also check the [command line reference](../command-line-reference/publish-test-results-command.md) for the `publish-test-results` command.

The following example shows the most common options within this section.

```javascript
{
  ...
  "publishTestResults": {
    "testResult": {
      "filePath": "test-result.trx"
    },
    "treatInconclusiveAs": "Failed",
    "testConfiguration": {
      "name": "Windows 10"
    },
    "testRunSettings": {
      "name": "Acceptance test results"
    },
    "testResultSettings": {
      "comment": "BDD results"
    }
  },
  ...
}
```

## Settings


| Setting | Description | Default |
| ------- | ----------- | ------- |
| `testResult` | <p>The test result configuration</p><ul><li><code>testResult/filePath</code> &#x2014; The path of the test result file (e.g. TRX) file or a folder containing multiple test result files.</li><li><code>testResult/fileFormat</code> &#x2014; The format of the test result file. Please check the <a href="../compatibility.md#supported-test-result-formats">Compatibility</a> page for supported formats. Invoking the <code>publish-test-result</code> command with <code>?</code> as format as command line option will list all supported format as well.</li></ul> | specified as [command line option](../command-line-reference/publish-test-results-command.md) |
| `treatInconclusiveAs` | Maps the Inconclusive test results. Some test execution frameworks report skipped scenarios as Inconclusive, so they should be mapped to another value, e.g. `NotExecuted`. | not mapped |
| `includeNotExecutedTests` | Includes test results that are not executed (outcomes `NotExecuted`, `NotApplicable`, `NotRunnable`, `NotImpacted`). | `false` |
| `testConfiguration` | <p>Specifies a test configuration within the Azure DevOps project as a target configuration for publishing test results.</p><ul><li><code>testConfiguration/name</code> &#x2014; The name of the test configuration.</li><li><code>testConfiguration/id</code> &#x2014; The ID of the test configuration.</li></ul><p>Can be overridden using with a <a href="../command-line-reference/publish-test-results-command.md">command line option</a>.</p> | uses the single Test Configuration assigned to the Test Suite |
| `testSuite` | <p>Specifies a test suite within the Azure DevOps project to publish the test results for.</p><ul><li><code>testSuite/name</code> &#x2014; The name of the test suite. For suites with non-unique names, please use the <code>testSuite/id</code> setting.</li><li><code>testSuite/id</code> &#x2014; The ID of the test suite as a number (e.g. <code>id: 12345</code>).</li><li><code>testSuite/testPlanId</code> &#x2014;The ID of the test plan to search or create the test suite in. (Optional, improves performance)</li></ul> | test cases are not included to a test suite |
| `testRunSettings` | <p>Specifies additional settings for the created test run.</p><ul><li><code>testRunSettings/name</code> &#x2014; The name of the created Test Run. (Default: [load from test result file])</li><li><code>testRunSettings/comment</code> &#x2014; The comment of the created Test Run. (Default: empty)</li><li><code>testRunSettings/runType</code> &#x2014;Sets the run type of the created Test Run. Possible values: `manual`, `automated`. (Default: set to `automated` when [`synchronization/automation/enabled`](configuration-synchronization/configuration-synchronization-automation.md) is `true`)</li></ul> | use default settings |
| `testResultSettings` | <p>Specifies additional settings for the created test results.</p><ul><li><code>testResultSettings/comment</code> &#x2014; The comment added to the individual test results within the created Test Run. (Default: empty)</li></ul> | use default settings |


{% page-ref page="./" %}

{% page-ref page="../command-line-reference/publish-test-results-command.md" %}

