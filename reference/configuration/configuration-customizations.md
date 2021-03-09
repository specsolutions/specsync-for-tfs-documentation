# customizations

This configuration section contains settings for configuring customizations.

{% hint style="info" %}
The customizations described here are [Enterprise features](../../licensing.md).
{% endhint %}

The following example shows the available options within this section.

```javascript
{
  ...
 "customizations": {
    "fieldDefaults": {
      "enabled": true,
      "defaultValues": {
        "MyCompany.MyCustomField": "Default 1",
        "MyCompany.OtherCustomField": "Default 2"
      }
    },
    "customFieldUpdates": {
      "enabled": true,
      "updates": {
        "System.Description": "Syncronized from feature {feature-name}{br}{feature-description}"
      }
    },
    "ignoreTestCaseSteps": {
      "enabled": true,
      "ignoredPrefixes": [ "COMMENT" ]
    },
    "ignoreTestCaseTags": {
      "enabled": true,
      "tags": [ "mytag", "ado-tag*" ]
    },
    "tagTextMapTransformation": {
      "enabled": true,
      "textMap": {
        "_": " "
      }
    },
    "multiSuitePublishTestResults": {
      "enabled": true,
      "testPlanId": 567,
      "suites": [
        "SuiteA",
        "SuiteB",
        "#3456"
      ],
      "includeSubSuites": true,
      "publishToRequirementBasedTestSuites": true,
      "linkTagPrefixes": [ "bug" ]
    },
    "branchTag": {
      "enabled": true,
      "prefix": "tc.mybranch"
    },
    "resetTestCaseState": {
      "enabled": true,
      "state": "Ready",
      "condition": "@ready"
    }
  }
  ...
}
```

## Available customizations

### fieldDefaults

Enables setting default values to test case fields. Useful for custom Azure DevOps process templates. See [Customization: Setting Test Case fields with default values](../../features/push-features/customization-setting-test-case-fields-with-default-values.md) for details.

| Setting | Description | Default |
| :--- | :--- | :--- |
| `fieldDefaults/enabled` | Enables the customization. | `false` |
| `fieldDefaults/defaultValues` | A list of key-value pair, where the key is the canonical name of the field to be updated \(e.g. `System.Description`\) and the value is the default value to be used when the test case is created.  | mandatory |

### customFieldUpdates

Enables updating test case fields that are normally not changed by SpecSync. See [Customization: Update custom Test Case fields on push](../../features/push-features/customization-update-custom-test-case-fields-on-push.md) for details.

| Setting | Description | Default |
| :--- | :--- | :--- |
| `customFieldUpdates/enabled` | Enables the customization. | `false` |
| `customFieldUpdates/updates` | A list of key-value pair, where the key is the canonical name of the field to be updated \(e.g. `System.Description`\) and the value is the template to be used to update the field. The template can contain placeholders listed in [Customization: Update custom Test Case fields on push - Template placeholders](../../features/push-features/customization-update-custom-test-case-fields-on-push.md#template-placeholders) page. | mandatory |

### ignoreTestCaseSteps

Can ignore \(leave unchanged\) test case steps with a specific prefix. See [Customization: Ignoring marked Test Case steps](../../features/push-features/customization-ignoring-marked-test-case-steps.md) for details.

| Setting | Description | Default |
| :--- | :--- | :--- |
| `ignoreTestCaseSteps/enabled` | Enables the customization. | `false` |
| `ignoreTestCaseSteps/ignoredPrefixes` | An array of prefixes. The test case steps that start with any of the listed prefixes \(case-insensitive\) will be ignored by the synchronization. | mandatory |

### ignoreTestCaseTags

Can ignore \(leave unchanged\) test case tags. See [Customization: Ignoring Test Case Tags](../../features/push-features/customization-ignoring-test-case-tags.md) for details.

| Setting | Description | Default |
| :--- | :--- | :--- |
| `ignoreTestCaseTags/enabled` | Enables the customization. | `false` |
| `ignoreTestCaseTags/tags` | An array of tag specifiers. The tag specifier can be a tag \(e.g. `mytag`\) or a tag prefix with tailing wildcard \(e.g. `ado-tag*` - ignores tags like `ado-tag-important`\). The test case tags that match to any of the listed tag specifiers will be ignored by the synchronization. | mandatory |

### 

### tagTextMapTransformation

Can substitute characters or sub-strings in tags when synchronizing to Azure DevOps. E.g. underscores \(`_`\) in scenario tags can be represented with spaces in Test Case tags. See [Customization: Mapping tags](../../features/push-features/customization-mapping-tags.md) for details.

| Setting | Description | Default |
| :--- | :--- | :--- |
| `tagTextMapTransformation/enabled` | Enables the customization. | `false` |
| `tagTextMapTransformation/textMap` | Character or substring replacement rules in 'X':'Y' format, where 'X' is a substring in Gherkin tag and 'Y' is a substring in Azure DevOps tag. | mandatory |

### multiSuitePublishTestResults

Allows publishing test results to multiple Test Suites. See [Customization: Publishing test results to multiple Test Suites](../../features/test-result-publishing-features/customization-publishing-test-results-to-multiple-test-suite.md) for details.

| Setting | Description | Default |
| :--- | :--- | :--- |
| `multiSuitePublishTestResults/enabled` | Enables the customization. | `false` |
| `multiSuitePublishTestResults/ testPlanId` | The ID of the test plan to search the test suites in. | mandatory |
| `multiSuitePublishTestResults/ publishToAllSuites` | When set to `true` SpecSync will publish the results to all test suites within the specified test plan. | `false` |
| `multiSuitePublishTestResults/suites` | The list of test suites to additionally publish the test results to. Test suite names or IDs \(like `"#1234"`\) can be specified. | empty list |
| `multiSuitePublishTestResults/ includeSubSuites` | When set to `true`, the results will be published not only to the specified suites, but also their direct or indirect sub-suites. | `false` |
| `multiSuitePublishTestResults/ publishToRequirementBasedTestSuites` | When set to `true`, the results will also be published to the requirement-based suites of the work items linked to the test case. The considered link prefixes can be restricted using the `linkTagPrefixes` setting. | `false` |
| `multiSuitePublishTestResults/ linkTagPrefixes` | Restricts the work item links to be considered for `publishToRequirementBasedTestSuites`. | all links are considered |

### branchTag

Supports synchronization of scenarios on feature branches. See [Customization: Synchronizing scenarios from feature branches](../../features/push-features/support-synchronizing-scenarios-from-a-branch.md) for details.

| Setting | Description | Default |
| :--- | :--- | :--- |
| `branchTag/enabled` | Enables the customization. | `false` |
| `branchTag/prefix` | The tag prefix to be used for linking scenarios that are updated on a branch. E.g. the prefix `tc.mybranch` will generate tags, like `@tc.mybranch:1234`. | mandatory |

### resetTestCaseState

Allows resetting Test Case state after change as a separate work item update based on tags. See [Customization: Reset Test Case state after change](../../features/push-features/customization-reset-test-case-state-after-change.md) for details.

| Setting | Description | Default |
| :--- | :--- | :--- |
| `resetTestCaseState/enabled` | Enables the customization. | `false` |
| `resetTestCaseState/state` | A state value \(e.g. `Ready`\) to set test case state to after updating a test case as a separate update. | mandatory |
| `resetTestCaseState/condition` | A [tag expression](http://speclink.me/tagexpressions) of scenarios that should be included for state change \(e.g. `@ready`, `not @inprogress`\). | all scenarios included for state change |

### linkOnChange

Allows linking changed Test Cases to a work item or pull request, related to the change. See [Customization: Automatically link changed Test Cases](../../features/push-features/customization-automatically-link-changed-test-cases.md) for details.

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
      <td style="text-align:left"><code>linkOnChange/enabled</code>
      </td>
      <td style="text-align:left">Enables the customization.</td>
      <td style="text-align:left"><code>false</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>linkOnChange/targetId</code>
      </td>
      <td style="text-align:left">The ID of the work item or pull request to link the Test Case to. Placeholders,
        like <code>{env:ENVIRONMENT_VARIABLE}</code> can be used.</td>
      <td style="text-align:left">mandatory</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>linkOnChange/relationship</code>
      </td>
      <td style="text-align:left">
        <p>Specify the relationship for the created link. E.g. specifying <code>Parent</code> means
          that the linked work item will be the parent of the test case work item.</p>
        <p>For linking Pull Requests it has to be set to <code>Pull Request</code>.</p>
      </td>
      <td style="text-align:left"><code>Tests</code>
      </td>
    </tr>
  </tbody>
</table>

{% page-ref page="./" %}

