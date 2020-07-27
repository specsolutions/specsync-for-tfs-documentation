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

* `enabled` -- Specifies whether SpecSync should attempt to create automated test cases. \(Default: `false`\)
* `skipForTags` -- A [tag expression](http://speclink.me/tagexpressions) of scenarios that should be excluded from automation \(e.g. `@manual or @planned`\). \(Default: _\[all test cases synced as automated\]_\)
* `testExecutionStrategy` -- Specifies the test execution strategy for the automated test cases. Check [Synchronizing automated test cases](../../../important-concepts/synchronizing-automated-test-cases.md) for details about the execution strategies. Available options: `assemblyBasedExecution`, `testSuiteBasedExecution`, `testSuiteBasedExecutionWithScenarioOutlineWrappers` and `none` \(Default: not set\)

_\[Back to the_ [`synchronization` _Configuration_](./)_\]_

_\[Back to the_ [_Configuration guide_](../)_\]_

