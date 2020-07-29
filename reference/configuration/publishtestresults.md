# publishTestResults

This configuration section contains settings related to publishing test results. 

To read more about publishing test results see the "Assembly based execution strategy" section of the [Synchronizing automated test cases](../../important-concepts/synchronizing-automated-test-cases.md) article. Please also check the [command line reference](../command-line-reference/publish-test-results-command.md) for the `publish-test-results` command.

The following example shows the most common options within this section.

```javascript
{
  ...
  "publishTestResults": {
    "testConfiguration": {
      "name": "Windows 10"
    },
    "testResult": {
      "filePath": "test-result.trx"
    },
    "runComment": "Acceptance test results"
  },
  ...
}
```

## Settings

<table>
  <thead>
    <tr>
      <th style="text-align:left">Setting</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>testConfiguration</code>
      </td>
      <td style="text-align:left">
        <p>Specifies a test configuration within the Azure DevOps project as a target
          configuration for publishing test results.</p>
        <ul>
          <li><code>testConfiguration/name</code> &#x2014; The name of the test configuration.</li>
          <li><code>testConfiguration/id</code> &#x2014; The ID of the test configuration.</li>
        </ul>
      </td>
      <td style="text-align:left">specified as <a href="../command-line-reference/publish-test-results-command.md">command line option</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>testResult</code>
      </td>
      <td style="text-align:left">
        <p>The test result configuration</p>
        <ul>
          <li><code>testResult/filePath</code> &#x2014; The path of the test result file
            (e.g. TRX) file.</li>
          <li><code>testResult/fileFormat</code> &#x2014; The format of the test result
            file. Please check the <a href="../compatibility.md#supported-test-result-formats">Compatibility</a> page
            for supported formats. Invoking the <code>publish-test-result</code> command
            with <code>?</code> as format as command line option will list all supported
            format as well.</li>
        </ul>
      </td>
      <td style="text-align:left">specified as <a href="../command-line-reference/publish-test-results-command.md">command line option</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>treatInconclusiveAs</code>
      </td>
      <td style="text-align:left">Maps the Inconclusive test results. Some test execution frameworks report
        skipped scenarios as Inconclusive, so they should be mapped to another
        value, e.g. <code>NotExecuted</code>.</td>
      <td style="text-align:left">not mapped</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>runName</code>
      </td>
      <td style="text-align:left">The name of the created Test Run.</td>
      <td style="text-align:left">load from test result file</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>runComment</code>
      </td>
      <td style="text-align:left">The comment of the created Test Run.</td>
      <td style="text-align:left">not set</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>testResultComment</code>
      </td>
      <td style="text-align:left">The comment added to the individual test results within the created Test
        Run.</td>
      <td style="text-align:left">not set</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>testSuite</code>
      </td>
      <td style="text-align:left">
        <p>Specifies a test suite within the Azure DevOps project to publish the
          test results for.</p>
        <ul>
          <li><code>testSuite/name</code> &#x2014; The name of the test suite. For suites
            with non-unique names, please use the <code>testSuite/id</code> setting.</li>
          <li><code>testSuite/id</code> &#x2014; The ID of the test suite as a number
            (e.g. <code>id: 12345</code>).</li>
          <li><code>testSuite/testPlanId</code> &#x2014;The ID of the test plan to search
            or create the test suite in. (Optional, improves performance)</li>
        </ul>
      </td>
      <td style="text-align:left">test cases are not included to a test suite</td>
    </tr>
  </tbody>
</table>

{% page-ref page="./" %}

{% page-ref page="../command-line-reference/publish-test-results-command.md" %}

