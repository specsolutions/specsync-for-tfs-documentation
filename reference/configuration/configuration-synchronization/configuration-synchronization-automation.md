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
      "skipForTags": "@manual",
      "testExecutionStrategy": "assemblyBasedExecution"
    },
    ...
  },
  ...
}
```

## Settings



| Setting | Description | Default |
| :--- | :--- | :--- |
| `enabled` | Specifies whether SpecSync should attempt to create automated test cases. | `false` |
| `skipForTags` | A [tag expression](http://speclink.me/tagexpressions) of scenarios that should be excluded from automation \(e.g. `@manual or @planned`\). | no scenarios excluded |
| `testExecutionStrategy` | Specifies the test execution strategy for the automated test cases. Check [Synchronizing automated test cases](../../../important-concepts/synchronizing-automated-test-cases.md) for details about the execution strategies. Available options: `assemblyBasedExecution`, `testSuiteBasedExecution`, `testSuiteBasedExecutionWithScenarioOutlineWrappers` and `none` | not set |

{% page-ref page="./" %}

{% page-ref page="../" %}

