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
      "testExecutionStrategy": "assemblyBasedExecution",
      "automatedTestType": "BDD tests"
    },
    ...
  },
  ...
}
```

{% hint style="info" %}
After changing the `condition` setting of the automation configuration, you need to perform the push command with an additional `--force` setting otherwise the existing Test Cases will not be updated until the next change of their scenario. Using the `--force` setting is only required once.
{% endhint %}

## Settings



| Setting                 | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Default                                 |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------- |
| `enabled`               | Specifies whether SpecSync should attempt to create automated test cases.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | `false`                                 |
| `condition`             | <p>A <a href="../../../features/general-features/local-test-case-conditions.md">local test case condition</a> of scenarios that should be included in automation (e.g. <code>not @manual or @automated</code>).</p><p>Before v3.2 this setting had to be specified as <code>skipForTags</code> where the excluded scenarios had to be specified.</p><p>After changing this setting, you need to use the `--force` option once to update existing Test Cases. See note above.</p>                                                                                                                                                                                                                                                                                                                                                                                       | all scenarios included                  |
| `automatedTestType`     | Specifies the value to be used in the _Automated test type_ field of the Test Case.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | uses a strategy-dependent default value |
| `testExecutionStrategy` | <p>Specifies the test execution strategy for the automated test cases. The <code>assemblyBasedExecution</code> strategy can be used to document that the tests are executed by running them from .NET assemblies. The <code>custom</code> strategy can be used for non .NET projects. </p><p>The options <code>testSuiteBasedExecution</code> and <code>testSuiteBasedExecutionWithScenarioOutlineWrappers</code> should only be used for suite based execution. See <a href="../../../features/test-result-publishing-features/support-for-azure-devops-test-plan-test-suite-based-test-execution.md">Support for Azure DevOps Test Plan / Test Suite based test execution</a> for details.</p> | `assemblyBasedExecution`                |



{% content-ref url="./" %}
[.](./)
{% endcontent-ref %}

{% content-ref url="../" %}
[..](../)
{% endcontent-ref %}
