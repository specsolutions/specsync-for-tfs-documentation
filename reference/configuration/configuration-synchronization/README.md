# synchronization

This configuration section contains synchronization settings.

The following example shows the available options within this section.

```javascript
{
  ...
  "synchronization": {
    "disableLocalChanges": false,
    "testCaseTagPrefix": "tc",

    "pull": {
      "enabled": true,
      "enableCreatingScenariosForNewTestCases": false
    },

    "automation": {
      "enabled": true,
      "condition": "not @manual"
    },

    "state": {
      "setValueOnChangeTo": "Design"
    },

    "areaPath": {
      "mode": "setOnLink",
      "value": "\\MyArea"
    },
    "iterationPath": {
      "mode": "setOnLink",
      "value": "\\Iteration1"
    },
    "links": [
      {
        "targetWorkItemType": "ProductBacklogItem",
        "tagPrefix": "pbi",
        "relationship": "Child",
        "mode": "createIfMissing"
      },
      {
        "tagPrefix": "bug"
      }
    ],
    "format": {
      "useExpectedResult": false,
      "syncDataTableAsText": false,
      "prefixBackgroundSteps": true,
      "prefixTitle": true
    }
  },
  ...
}
```

## Settings

For sub-section settings, click on the name of the setting for details.

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
      <td style="text-align:left"><code>disableLocalChanges</code>
      </td>
      <td style="text-align:left">Disables changing feature files in the local repository. If set to true
        (called <em>build server mode</em>), only those changes will be performed
        that do not need any change in the local feature files. Linking new scenarios
        or pulling changes from test cases will be skipped. Can be overridden by
        the <code>--disableLocalChanges</code>  <a href="../../command-line-reference/push-command.md">command line option</a>.
        See <a href="../../../important-concepts/synchronizing-test-cases-from-build.md">Synchronizing test cases from build</a> for
        details.</td>
      <td style="text-align:left"><code>false</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>testCaseTagPrefix</code>
      </td>
      <td style="text-align:left">
        <p></p>
        <p>The tag prefix for specifying the test cases. E.g. specify <code>testcase</code> for
          referring to test cases using a tag, like <code>@testcase:1234</code>.</p>
      </td>
      <td style="text-align:left"><code>tc</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;code&gt;&lt;/code&gt;<a href="configuration-synchronization-pull.md"><code>pull</code></a>&lt;code&gt;&lt;/code&gt;</td>
      <td
      style="text-align:left">Settings to configure the pull behavior. See <a href="../../../features/pull-features/two-way-synchronization.md">Two-way synchronization</a> for
        details</td>
        <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;code&gt;&lt;/code&gt;<a href="configuration-synchronization-automation.md"><code>automation</code></a>&lt;code&gt;&lt;/code&gt;</td>
      <td
      style="text-align:left">Settings to configure synchronizing automated test cases. See <a href="../../../important-concepts/synchronizing-automated-test-cases.md">Synchronizing automated test cases</a> for
        details.</td>
        <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;code&gt;&lt;/code&gt;<a href="configuration-synchronization-state.md"><code>state</code></a>&lt;code&gt;&lt;/code&gt;</td>
      <td
      style="text-align:left">Settings to configure how the test case <em>state</em> field should be updated.</td>
        <td
        style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;code&gt;&lt;/code&gt;<a href="configuration-synchronization-areapath.md"><code>areaPath</code></a>&lt;code&gt;&lt;/code&gt;</td>
      <td
      style="text-align:left">Settings to configure how the test case <em>area path</em> field should
        be updated. See <a href="../../../features/push-features/add-new-test-cases-to-an-area-or-an-iteration.md">Add new test cases to an Area or an Iteration</a> for
        details.</td>
        <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;code&gt;&lt;/code&gt;<a href="configuration-synchronization-iterationpath.md"><code>iterationPath</code></a>&lt;code&gt;&lt;/code&gt;</td>
      <td
      style="text-align:left">Settings to configure how the test case <em>iteration path</em> field should
        be updated. See <a href="../../../features/push-features/add-new-test-cases-to-an-area-or-an-iteration.md">Add new test cases to an Area or an Iteration</a> for
        details.</td>
        <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;code&gt;&lt;/code&gt;<a href="configuration-synchronization-links.md"><code>links</code></a>&lt;code&gt;&lt;/code&gt;</td>
      <td
      style="text-align:left">Settings to configure how test case links should be updated based on the
        tags of the scenario. See <a href="../../../features/common-synchronization-features/linking-work-items-with-tags.md">Linking work items using tags</a> for
        details.</td>
        <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;code&gt;&lt;/code&gt;<a href="configuration-synchronization-format.md"><code>format</code></a>&lt;code&gt;&lt;/code&gt;</td>
      <td
      style="text-align:left">Settings to configure the format of the synchronized test case. See
        <a
        href="../../../features/push-features/configuring-the-format-of-the-synchronized-test-cases.md">Configuring the format of the synchronized test cases</a>for details.</td>
          <td
          style="text-align:left"></td>
    </tr>
  </tbody>
</table>

{% page-ref page="../" %}

