# specFlow

This configuration section contains settings related to synchronizing SpecFlow projects. These settings are only required for old SpecFlow versions and for [synchronizing automated test cases](../../important-concepts/synchronizing-automated-test-cases.md).

The following example shows the available options within this section.

```javascript
{
  ...
  "specFlow": {
    "specFlowGeneratorFolder": "..\\packages\\SpecFlow.2.3.0\\tools",
    "generateFeatureFileCodeBehinds": false,
    "scenarioOutlineAutomationWrappers": "iterateThroughExamples",
    "wrapperMethodPrefix": "_SpecSyncWrapper_",
    "wrapperMethodCategory": "SpecSyncWrapper"
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
      <td style="text-align:left"><code>specFlowGeneratorFolder</code>
      </td>
      <td style="text-align:left">The path of the SpecFlow generator folder used by the project, that is
        usually the <code>tools</code> folder of the SpecFlow NuGet package, e.g. <code>packages\\SpecFlow.2.3.0\\tools</code>.</td>
      <td
      style="text-align:left">detect generator of the project</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>generateFeatureFileCodeBehinds</code>
      </td>
      <td style="text-align:left">Specifies whether SpecSync should attempt to regenerate feature file code-behind
        files after they have been changed by SpecSync. This is only required for
        SpecFlow v2.</td>
      <td style="text-align:left">SpecSync automatically decides whether generation is necessary</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>scenarioOutlineAutomationWrappers</code>
      </td>
      <td style="text-align:left">
        <p>Specifies how automation wrapper methods should be generated for synchronizing
          scenario outlines to automated test cases. Available options: <code>useTestCaseData</code> and <code>iterateThroughExamples</code>.
          (Default: <code>iterateThroughExamples</code>)</p>
        <ul>
          <li><code>useTestCaseData</code> -- the generated wrapper method loads the
            test data for the iterations from the test case. Running the test cases
            through this wrapper in Azure DevOps generates a detailed report about
            each iteration, but it cannot be executed locally and also does not work
            in from Azure DevOps pipeline build. See <a href="../../important-concepts/synchronizing-automated-test-cases.md#use-testcase-data-for-scenario-outline-examples-for-legacy-mstest-v1-projects">related section of the Synchronizing automated test cases article</a> for
            details.</li>
          <li><code>iterateThroughExamples</code> -- the generated wrapper method iterates
            through the examples and runs the test for each. A failure of an iteration
            does not block the remaining iterations. Running the test cases through
            this wrapper in Azure DevOps generates a single entry in the report, but
            the details of the entry contain all executed data set.</li>
        </ul>
      </td>
      <td style="text-align:left"><code>iterateThroughExamples</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>wrapperMethodPrefix</code>
      </td>
      <td style="text-align:left">The method prefix to be used for the generated automation wrapper methods.</td>
      <td
      style="text-align:left"><code>_SpecSyncWrapper_</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>wrapperMethodCategory</code>
      </td>
      <td style="text-align:left">The test category (trait) be added for the generated automation wrapper
        methods.</td>
      <td style="text-align:left"><code>SpecSyncWrapper</code>
      </td>
    </tr>
  </tbody>
</table>

{% page-ref page="./" %}

