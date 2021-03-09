# publish-test-results

Publishes local test results to Azure DevOps server as a Test Run, where the results are connected to the synchronized test cases and optionally to a build.

See more details about the command in the "Assembly based execution strategy" section of the [Synchronizing automated test cases](../../important-concepts/synchronizing-automated-test-cases.md) article.

## Options

In addition the the options listed here, all [common command line options](./#common-command-line-options) can also be used.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Option</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><code>--testConfiguration </code>&lt;CONFIGURATION&gt;</p>
        <p>(short name <code>-c</code>)</p>
      </td>
      <td style="text-align:left">The Azure DevOps Test Configuration name or ID to publish the results
        for. For specifying an ID, use <code>#1234</code> format.</td>
      <td style="text-align:left">use from config file or detect automatically</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><code>--testResultFile</code> &lt;FILE&#x2011;PATH&gt;</p>
        <p>(short name <code>-r</code>)</p>
      </td>
      <td style="text-align:left">
        <p>The file path of the test result (.trx, .xml or .json) file to publish
          or a folder that contains multiple test result files.</p>
        <p>Multiple paths can be listed, separated by semicolon (<code>;</code>).</p>
      </td>
      <td style="text-align:left">use from config file</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><code>--testResultFileFormat</code> &lt;FORMAT&gt;</p>
        <p>(short name <code>-f</code>)</p>
      </td>
      <td style="text-align:left">The file format of the file to publish. Please check the <a href="../compatibility.md#supported-test-result-formats">Compatibility</a> page
        for supported formats. Invoking the command with <code>?</code> as format
        will list all supported format as well.</td>
      <td style="text-align:left">use from config file or detect automatically</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--testSuite</code> &lt;SUITE&#x2011;NAME&#x2011;OR&#x2011;ID&gt;</td>
      <td
      style="text-align:left">A Test Suite name or ID to publish the test results to. For specifying
        an ID, use <code>#1234</code> format. (e.g. <code>My Suite</code> or <code>#1234</code>)</td>
        <td
        style="text-align:left">use from config file</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--testPlanId </code>&lt;PLAN&#x2011;ID&gt;</td>
      <td style="text-align:left">The ID of the Test Plan to search the Test Suite in. (e.g. <code>123</code>)</td>
      <td
      style="text-align:left">all Test Plans are scanned through</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--runName</code> &lt;NAME&gt;</td>
      <td style="text-align:left">The name of the Test Run to be created.</td>
      <td style="text-align:left">get from test result file</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--runComment</code> &lt;RUN&#x2011;COMMENT&gt;</td>
      <td style="text-align:left">The comment field of the Test Run to be created.</td>
      <td style="text-align:left">not specified</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--testResultComment</code> &lt;RESULT&#x2011;COMMENT&gt;</td>
      <td
      style="text-align:left">The comment added to the individual test results within the created test
        run. Useful if the individual test results are typically browsed not through
        the Test Run.</td>
        <td style="text-align:left">not specified</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--buildId</code> &lt;BUILD&#x2011;ID&gt;</td>
      <td style="text-align:left">The build ID (e.g. <code>345</code>) of the build the test result was created
        for. To prevent detecting build from build, it can be set to an empty value
        (<code>--buildId &quot; &quot;</code>).</td>
      <td style="text-align:left">detect from current build</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--buildNumber</code> &lt;BUILD&#x2011;NUMBER&gt;</td>
      <td style="text-align:left">The build number (e.g. <code>20200119.1</code>) of the build the test result
        was created for. Should be specified when build ID is not known.</td>
      <td
      style="text-align:left">build ID is used</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--buildFlavor</code> &lt;FLAVOR&gt;</td>
      <td style="text-align:left">The build flavor (e.g. <code>Debug</code>) of the build the test result
        was created for. Can only be specified if either <code>--buildNumber</code> or <code>--buildId</code> is
        specified.</td>
      <td style="text-align:left">detect from current build</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--buildPlatform</code> &lt;PLATFORM&gt;</td>
      <td style="text-align:left">he build flavor (e.g. <code>x86</code>) of the build the test result was
        created for. Can only be specified if either <code>--buildNumber</code> or <code>--buildId</code> is
        specified.</td>
      <td style="text-align:left">detect from current build</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--attachedFiles</code> &lt;FILE&#x2011;LIST&gt;</td>
      <td style="text-align:left">Semicolon separated list of file paths that should be attached to the
        test run additionally. (e.g. <code>error1.png;error2.log</code>) Wildcards
        are currently not supported.</td>
      <td style="text-align:left">only test result file attached</td>
    </tr>
  </tbody>
</table>

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

