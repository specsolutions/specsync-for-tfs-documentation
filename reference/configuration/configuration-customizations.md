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
    }
  }
  ...
}
```

## Available customizations

### fieldDefaults

Enables setting default values to test case fields. Useful for custom Azure DevOps process templates.

| Setting | Description | Default |
| :--- | :--- | :--- |
| `fieldDefaults/enabled` | Enables the customization. | `false` |
| `fieldDefaults/defaultValues` | A list of key-value pair, where the key is the canonical name of the field to be updated \(e.g. `System.Description`\) and the value is the default value to be used when the test case is created.  | mandatory |

### customFieldUpdates

Enables updating test case fields that are normally not changed by SpecSync.

| Setting | Description | Default |
| :--- | :--- | :--- |
| `customFieldUpdates/enabled` | Enables the customization. | `false` |
| `customFieldUpdates/updates` | A list of key-value pair, where the key is the canonical name of the field to be updated \(e.g. `System.Description`\) and the value is the template to be used to update the field. The template can contain placeholders listed below. | mandatory |

The template value for `customFieldUpdates/updates` can contain the following placeholders:

* `{feature-name}` -- the name of the feature \(specified in the feature file header\)
* `{feature-description}` -- the description of the feature \(the free-text block specified after the feature file header\)
* `{scenario-name}` -- the name of the scenario or scenario outline
* `{scenario-description}` -- the description of the scenario or scenario outline 
* `{feature-file-name}` -- the file name of the feature file \(without folder\)
* `{feature-file-folder}` -- the folder of the feature file, relative to the project root
* `{feature-file-path}` -- the path \(folder and file name\) of the feature file, relative to the project root
* `{br}` -- a new line

### ignoreTestCaseSteps

Can ignore \(leave unchanged\) test case steps with a specific prefix.

| Setting | Description | Default |
| :--- | :--- | :--- |
| `ignoreTestCaseSteps/enabled` | Enables the customization. | `false` |
| `ignoreTestCaseSteps/ignoredPrefixes` | An array of prefixes. The test case steps that start with any of the listed prefixes \(case-insensitive\) will be ignored by the synchronization. | mandatory |

### tagTextMapTransformation

Can substitute characters or sub-strings in tags when synchronizing to Azure DevOps. E.g. underscores \(`_`\) in scenario tags can be represented with spaces in Test Case tags.

| Setting | Description | Default |
| :--- | :--- | :--- |
| `tagTextMapTransformation/enabled` | Enables the customization. | `false` |
| `tagTextMapTransformation/textMap` | Character or substring replacement rules in 'X':'Y' format, where 'X' is a substring in Gherkin tag and 'Y' is a substring in Azure DevOps tag. | mandatory |

### multiSuitePublishTestResults

Allows publishing test results to multiple Test Suites.

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

Supports synchronization of scenarios on feature branches.

| Setting | Description | Default |
| :--- | :--- | :--- |
| `branchTag/enabled` | Enables the customization. | `false` |
| `branchTag/prefix` | The tag prefix to be used for linking scenarios that are updated on a branch. E.g. the prefix `tc.mybranch` will generate tags, like `@tc.mybranch:1234`. | mandatory |

{% page-ref page="./" %}

