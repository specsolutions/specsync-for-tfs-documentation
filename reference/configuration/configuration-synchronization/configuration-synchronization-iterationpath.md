# iterationPath

This configuration section contains settings to configure how the test case _iteration path_ field should be updated.

Read more about the setting the _area path_ and _iteration path_ fields in the [Add new test cases to an Area or an Iteration](../../../features/push-features/add-new-test-cases-to-an-area-or-an-iteration.md) concept description.

The following example shows the available options within this sub-section.

```javascript
{
  ...
  "synchronization": {
    ...
    "iterationPath": {
      "mode": "setOnLink",
      "value": "\\Iteration1"
    },
    ...
  },
  ...
}
```

The `iterationPath` setting is a shortcut for configuring the [`synchronization/fieldUpdates` section](configuration-synchronization-fieldupdates.md). The example above is equivalent to the following `fieldUpdates` setting:

```javascript
{
  "synchronization": {
    "fieldUpdates": {
      "System.IterationPath": {
        "value": "project-name\\Iteration1",
        "update": "onCreate"
      }
    }
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
      <td style="text-align:left"><code>mode</code>
      </td>
      <td style="text-align:left">
        <p>Specifies how the iteration path of the test case should be updated. Available
          options: <code>notSet</code> and <code>setOnLink</code>. (Default: <code>notSet</code>)</p>
        <ul>
          <li><code>notSet</code>: the path is not set</li>
          <li><code>setOnLink</code>: set the path when the test case created and linked
            to the scenario, but not to update later on.</li>
        </ul>
      </td>
      <td style="text-align:left"><code>notSet</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>value</code>
      </td>
      <td style="text-align:left">The iteration path to set for test cases (e.g. <code>\\Iteration1</code>).
        The project name prefix can be omitted.</td>
      <td style="text-align:left">mandatory for mode <code>setOnLink</code>
      </td>
    </tr>
  </tbody>
</table>

{% page-ref page="./" %}

{% page-ref page="../" %}

