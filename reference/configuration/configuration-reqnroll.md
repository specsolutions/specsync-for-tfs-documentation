# reqnroll

This configuration section contains settings related to synchronizing Reqnroll projects. These settings are only required for the legacy [synchronizing automated test cases](../../important-concepts/synchronizing-automated-test-cases.md)

The following example shows the available options within this section.

```javascript
{
  ...
  "reqnroll": {
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
      <td style="text-align:left"><code>scenarioOutlineAutomationWrappers</code>
      </td>
      <td style="text-align:left">
        <p>Specifies how automation wrapper methods should be generated for synchronizing
          scenario outlines to automated test cases. Available options: <code>iterateThroughExamples</code>.
          (Default: <code>iterateThroughExamples</code>)</p>
        <ul>
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

