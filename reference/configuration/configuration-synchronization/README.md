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
        "relationship": "Child"
      },
      {
        "tagPrefix": "bug"
      }
    ],
    "attachments": {
      "enabled": true,
      "tagPrefixes": [
        "wireframe",
        "specification"
      ],
      "baseFolder": "resources/files"
    },
    "format": {
      "useExpectedResult": false,
      "emptyActionValue": "N/A"
      "emptyExpectedResultValue": "N/A"
      "syncDataTableAsText": false,
      "prefixBackgroundSteps": true,
      "prefixTitle": true,
      "showParameterListStep": "whenUnusedParameters"
    }
  },
  ...
}
```

## Settings

For sub-section settings, click on the name of the setting for details.

| Setting | Description | Default |
| ----------------------- | ----------------------- | ----------------------- |
| `disableLocalChanges` | Disables changing feature files in the local repository. If set to true (called _build server mode_), only those changes will be performed that do not need any change in the local feature files. Linking new scenarios or pulling changes from test cases will be skipped. Can be overridden by the `--disableLocalChanges` [command line option](../../command-line-reference/push-command.md). See [Synchronizing test cases from build](../../../important-concepts/synchronizing-test-cases-from-build.md) for details. | `false` |
| `testCaseTagPrefix` | <p></p><p>The tag prefix for specifying the test cases. E.g. specify <code>testcase</code> for referring to test cases using a tag, like <code>@testcase:1234</code>.</p> | `tc` |
| `tagPrefixSeparators` | Specifies one or more tag prefix separators to recognize in order to be able to support links like `@story=42`. For creating tags, the first separator is used. | `[":"]` (only `:` can be used as separator) |
| `linkLabelSeparator` | Specifies the link label separator in order to be able to support links like `@story:42,this_is_the_title`. | `;` |
| [`pull`](configuration-synchronization-pull.md) | Settings to configure the pull behavior. See [Two-way synchronization](../../../features/pull-features/two-way-synchronization.md) for details |  |
| [`automation`](configuration-synchronization-automation.md) | Settings to configure synchronizing automated test cases. See [Synchronizing automated test cases](../../../important-concepts/synchronizing-automated-test-cases.md) for details. |  |
| [`state`](configuration-synchronization-state.md) | Settings to configure how the test case _state_ field should be updated. |  |
| [`areaPath`](configuration-synchronization-areapath.md) | Settings to configure how the test case _area path_ field should be updated. See [Add new test cases to an Area or an Iteration](../../../features/push-features/add-new-test-cases-to-an-area-or-an-iteration.md) for details. |  |
| [`iterationPath`](configuration-synchronization-iterationpath.md) | Settings to configure how the test case _iteration path_ field should be updated. See [Add new test cases to an Area or an Iteration](../../../features/push-features/add-new-test-cases-to-an-area-or-an-iteration.md) for details. |  |
| [`links`](configuration-synchronization-links.md) | Settings to configure how test case links should be updated based on the tags of the scenario. See [Linking work items using tags](../../../features/common-synchronization-features/linking-work-items-with-tags.md) for details. |  |
| [`attachments`](configuration-synchronization-attachments.md) | Settings to allow attaching files to Test Cases during synchronization using tags. See [Attach files to Test Cases using tags](../../../features/push-features/attach-files.md) for details. |  |
| [`format`](configuration-synchronization-format.md) | Settings to configure the format of the synchronized test case. See [Configuring the format of the synchronized test cases](../../../features/push-features/configuring-the-format-of-the-synchronized-test-cases.md) for details. |  |
| [`fieldUpdates`](configuration-synchronization-fieldupdates.md) | Specifies how the fields of the Test Case that are not managed by SpecSync should be updated. It can contain update rules in `"key": <VALUE>` format, where `key` is a field name or identifier and `<VALUE>` is the value to update to or an update specifier. See [Update Test Case fields](../../../features/push-features/update-test-case-fields.md) for details.|  |

{% content-ref url="../" %}
[..](../)
{% endcontent-ref %}
