# automation

This configuration section contains settings to configure synchronizing automated test cases.

Read more about synchronizing automated test cases in the [Synchronizing automated test cases](../../../important-concepts/synchronizing-automated-test-cases.md) concept description.

The following example shows the available options within this sub-section.

```javascript
{
  ...
  "synchronization": {
    ...
    "automation": {
      "enabled": true,
      "condition": "not @manual",
      "testExecutionStrategy": "assemblyBasedExecution"
    },
    ...
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
      <td style="text-align:left"><code>enabled</code>
      </td>
      <td style="text-align:left">Specifies whether SpecSync should attempt to create automated test cases.</td>
      <td
      style="text-align:left"><code>false</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>condition</code>
      </td>
      <td style="text-align:left">
        <p>A <a href="http://speclink.me/tagexpressions">tag expression</a> of scenarios
          that should be included in automation (e.g. <code>not @manual or @automated</code>).</p>
        <p>Before v3.2 this setting had to be specified as <code>skipForTags</code> where
          the excluded scenarios had to be specified.</p>
      </td>
      <td style="text-align:left">all scenarios included</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>testExecutionStrategy</code>
      </td>
      <td style="text-align:left">Specifies the test execution strategy for the automated test cases. The <code>assemblyBasedExecution</code>strategy
        (default from v3.1) is used in the most of the cases. The options <code>testSuiteBasedExecution</code> and <code>testSuiteBasedExecutionWithScenarioOutlineWrappers</code> should
        only be used for suite based execution. See <a href="../../../features/test-result-publishing-features/support-for-azure-devops-test-plan-test-suite-based-test-execution.md">Support for Azure DevOps Test Plan / Test Suite based test execution</a> for
        details.</td>
      <td style="text-align:left"><code>assemblyBasedExecution</code>
      </td>
    </tr>
  </tbody>
</table>

{% page-ref page="./" %}

{% page-ref page="../" %}

