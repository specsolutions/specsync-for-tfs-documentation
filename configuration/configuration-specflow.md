# specFlow

This configuration section contains settings related to synchronizing SpecFlow projects. These settings are only required for synchronizing automated test cases. See [Synchronizing automated test cases](../important-concepts/synchronizing-automated-test-cases.md) for more details.

The following example shows the available options within this section.

```javascript
{
  ...
  "specFlow": {
    "specFlowGeneratorFolder": "..\\packages\\SpecFlow.2.3.0\\tools",
    "scenarioOutlineAutomationWrappers": "iterateThroughExamples",
    "wrapperMethodPrefix": "_SpecSyncWrapper_",
    "wrapperMethodCategory": "SpecSyncWrapper"
  },
  ...
}
```

## Settings

* `specFlowGeneratorFolder` -- The path of the SpecFlow generator folder used by the project, that is usually the `tools` folder of the SpecFlow NuGet package, e.g. `packages\\SpecFlow.2.3.0\\tools`. \(Default: _\[detect generator of the project\]_\)
* `scenarioOutlineAutomationWrappers` -- Specifies how automation wrapper methods should be generated for synchronizing scenario outlines to automated test cases. Available options: `useTestCaseData` and `iterateThroughExamples`. \(Default: `iterateThroughExamples`\)
  * `useTestCaseData` -- the generated wrapper method loads the test data for the iterations from the test case. Running the test cases through this wrapper in Azure DevOps generates a detailed report about each iteration, but it cannot be executed locally and also does not work in from Azure DevOps pipeline build. See [related section of the Synchronizing automated test cases article](../important-concepts/synchronizing-automated-test-cases.md#use-testcase-data-for-scenario-outline-examples-for-legacy-mstest-v1-projects) for details.
  * `iterateThroughExamples` -- the generated wrapper method iterates through the examples and runs the test for each. A failure of an iteration does not block the remaining iterations. Running the test cases through this wrapper in Azure DevOps generates a single entry in the report, but the details of the entry contain all executed data set.
* `wrapperMethodPrefix` -- The method prefix to be used for the generated automation wrapper methods. \(Default: `_SpecSyncWrapper_`\)
* `wrapperMethodCategory` -- The test category \(trait\) be added for the generated automation wrapper methods. \(Default: `SpecSyncWrapper`\)

_\[Back to the_ [_Configuration guide_](./)_\]_

