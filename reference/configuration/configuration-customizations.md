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
    "ignoreNotSupportedLocalTags": {
      "enabled": true,
      "supportedTags": [ "@myTag", "@otherTag" ]
    }    
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
    "resetTestCaseState": {
      "enabled": true,
      "state": "Ready",
      "condition": "@ready"
    },
    "addTestCasesToSuites": {
      "enabled": true,
      "testSuites": [
        {
          "name": "Important Logic",
          "condition": "@important and not @external"
        }
      ]
    },
    "branchTag": {
      "enabled": true,
      "prefix": "tc.mybranch"
    },
    "linkOnChange": {
      "enabled": true,
      "links": [
        {
          "targetId": "{env:CURRENT_STORY}",
        }
      ]
    },
    "synchronizeLinkedArtifactTitles": {
      "enabled": true,
      "linkTagPrefixes": [ "story" ]
    },
    "doNotSynchronizeTitle": {
      "enabled": true
    }
  }
  ...
}
```

## Available customizations

### fieldDefaults

Enables setting default values to test case fields. Useful for custom Azure DevOps process templates. See [Customization: Setting Test Case fields with default values](../../features/push-features/customization-setting-test-case-fields-with-default-values.md) for details.

| Setting | Description | Default |
| ------- | ----------- | ------- |
| `fieldDefaults/enabled`       | Enables the customization.                                                                                                                                                                         | `false`   |
| `fieldDefaults/defaultValues` | A list of key-value pair, where the key is the canonical name of the field to be updated (e.g. `System.Description`) and the value is the default value to be used when the test case is created.  | mandatory |

### customFieldUpdates

Enables updating test case fields that are normally not changed by SpecSync. See [Customization: Update custom Test Case fields on push](../../features/push-features/customization-update-custom-test-case-fields-on-push.md) for details.

| Setting | Description | Default |
| ------- | ----------- | ------- |
| `customFieldUpdates/enabled` | Enables the customization.                                                                                                                                                                                                                                                                                                                                                                                                           | `false`   |
| `customFieldUpdates/updates` | A list of key-value pair, where the key is the canonical name of the field to be updated (e.g. `System.Description`) and the value is the template to be used to update the field. The template can contain placeholders listed in the [reference](../../reference/configuration/configuration-synchronization/configuration-synchronization-fieldupdates.md#update-placeholders). | mandatory |

### ignoreTestCaseSteps

Can ignore (leave unchanged) test case steps with a specific prefix. See [Customization: Ignoring marked Test Case steps](../../features/push-features/customization-ignoring-marked-test-case-steps.md) for details.

| Setting | Description | Default |
| ------- | ----------- | ------- |
| `ignoreTestCaseSteps/enabled`         | Enables the customization.                                                                                                                      | `false`   |
| `ignoreTestCaseSteps/ignoredPrefixes` | An array of prefixes. The test case steps that start with any of the listed prefixes (case-insensitive) will be ignored by the synchronization. | mandatory |

### ignoreTestCaseTags

Can ignore (leave unchanged) test case tags. See [Customization: Ignoring Test Case Tags](../../features/push-features/customization-ignoring-test-case-tags.md) for details.

| Setting | Description | Default |
| ------- | ----------- | ------- |
| `ignoreTestCaseTags/enabled` | Enables the customization.                                                                                                                                                                                                                                                           | `false`   |
| `ignoreTestCaseTags/tags`    | An array of tag specifiers. The tag specifier can be a tag (e.g. `mytag`) or a tag prefix with tailing wildcard (e.g. `ado-tag*` - ignores tags like `ado-tag-important`). The test case tags that match to any of the listed tag specifiers will be ignored by the synchronization. | mandatory |

### ignoreNotSupportedLocalTags

Can be used to specify supported tags. SpecSync will only synchronize the supported tags and ignore all others. See [customization-ignore-non-supported-local-tags.md](../../features/push-features/customization-ignore-non-supported-local-tags.md "mention") for details.

| Setting | Description | Default |
| ------- | ----------- | ------- |
| `ignoreNotSupportedLocalTags/enabled` | Enables the customization. | `false` |
| `ignoreNotSupportedLocalTags/supportedTags` | The list of local (scenario) tags that can be synchronized to Azure DevOps. The list can contain full tag names (e.g. `@my-tag1`) or tag name prefixes with tail wildcard (e.g. `@my-tag*`). | empty (no tags are supported)   |
| `ignoreNotSupportedLocalTags/notSupportedTags` | The list of local (scenario) tags that cannot be synchronized to Azure DevOps. This setting cannot be used together with 'supportedTags'. The list can contain full tag names (e.g. `@my-tag1`) or tag name prefixes with tail wildcard (e.g. `@my-tag*`). | `supportedTags` setting is used |

### tagTextMapTransformation

Can substitute characters or sub-strings in tags when synchronizing to Azure DevOps. E.g. underscores (`_`) in scenario tags can be represented with spaces in Test Case tags. See [Customization: Mapping tags](../../features/push-features/customization-mapping-tags.md) for details.

| Setting | Description | Default |
| ------- | ----------- | ------- |
| `tagTextMapTransformation/enabled` | Enables the customization. | `false` |
| `tagTextMapTransformation/textMap` | Character or substring replacement rules in 'X':'Y' format, where 'X' is a substring in Gherkin tag and 'Y' is a substring in Azure DevOps tag. | mandatory |

### multiSuitePublishTestResults

Allows publishing test results to multiple Test Suites. See [Customization: Publishing test results to multiple Test Suites](../../features/test-result-publishing-features/customization-publishing-test-results-to-multiple-test-suite.md) for details.

| Setting | Description | Default |
| ------- | ----------- | ------- |
| `multiSuitePublishTestResults/enabled` | Enables the customization. | `false` |
| `multiSuitePublishTestResults/testPlanId` | The ID of the test plan to search the test suites in. | mandatory |
| `multiSuitePublishTestResults/publishToAllSuites` | When set to `true` SpecSync will publish the results to all test suites within the specified test plan. | `false` |
| `multiSuitePublishTestResults/suites` | The list of test suites to additionally publish the test results to. | empty list |
| `multiSuitePublishTestResults/suites[]/name` | The name of the Test Suite | either `name`, `id` or `path` is mandatory |
| `multiSuitePublishTestResults/suites[]/id` | The ID of the Test Suite | either `name`, `id` or `path` is mandatory |
| `multiSuitePublishTestResults/suites[]/path` | The path of the Test Suite from the root of the Test Plan, separated by `/` (e.g. `Ordering/Card Payment`). | either `name`, `id` or `path` is mandatory |
| `multiSuitePublishTestResults/suites[]/testPlanId` | Deprecated, use 'testPlan' instead. | not specified |
| `multiSuitePublishTestResults/suites[]/testPlan` | The name or ID of the Test Plan to search or create the test suite in, e.g. `My Plan` or `#1234`. (Optional, improves performance) | not specified |
| `multiSuitePublishTestResults/includeSubSuites` | When set to `true`, the results will be published not only to the specified suites, but also their direct or indirect sub-suites. | `false` |
| `multiSuitePublishTestResults/publishToRequirementBasedTestSuites` | When set to `true`, the results will also be published to the requirement-based suites of the work items linked to the test case. The considered link prefixes can be restricted using the `linkTagPrefixes` setting. | `false` |
| `multiSuitePublishTestResults/linkTagPrefixes` | Restricts the work item links to be considered for `publishToRequirementBasedTestSuites`. | all links are considered |

### resetTestCaseState

Allows resetting Test Case state after change as a separate work item update based on tags. See [Customization: Reset Test Case state after change](../../features/push-features/customization-reset-test-case-state-after-change.md) for details.

| Setting | Description | Default |
| ------- | ----------- | ------- |
| `resetTestCaseState/enabled`   | Enables the customization.                                                                                                                      | `false`                                 |
| `resetTestCaseState/state`     | A state value (e.g. `Ready`) to set test case state to after updating a test case as a separate update.                                         | mandatory                               |
| `resetTestCaseState/condition` | A [local test case condition](../../features/general-features/local-test-case-conditions.md) of scenarios that should be included for state change (e.g. `@ready`, `not @inprogress`). | all scenarios included for state change |

### addTestCasesToSuites

Allows including the synchronized Test Cases into various static Test Suites based on conditions. See [Customization: Add Test Cases to Suites](../../features/push-features/customization-add-test-cases-to-suites.md) for details.

| Setting | Description | Default |
| ------- | ----------- | ------- |
| `addTestCasesToSuites/enabled` | Enables the customization. | `false` |
| `addTestCasesToSuites/testPlan` | The name or ID of the default Test Plan to search or create the test suites in. Can be overridden for specific suites. E.g. `My Plan` or `#1234`. | all test plans are scanned through |
| `addTestCasesToSuites/testSuites[]/name` | The name of the Test Suite | either `name`, `id` or `path` is mandatory |
| `addTestCasesToSuites/testSuites[]/id` | The ID of the Test Suite | either `name`, `id` or `path` is mandatory |
| `addTestCasesToSuites/testSuites[]/path` | The path of the Test Suite from the root of the Test Plan, separated by `/` (e.g. `Ordering/Card Payment`). | either `name`, `id` or `path` is mandatory |
| `addTestCasesToSuites/testSuites[]/testPlanId` | Deprecated, use 'testPlan' instead. | not specified |
| `addTestCasesToSuites/testSuites[]/testPlan` | The name or ID of the Test Plan to search or create the test suite in, e.g. `My Plan` or `#1234`. (Optional, improves performance) | not specified |
| `addTestCasesToSuites/testSuites[]/condition` | A [local test case condition](../../features/general-features/local-test-case-conditions.md) of scenarios for which the linked Test Case should be included in the Suite (e.g. `@ready`, `not @inprogress`). | all scenarios are considered |

### branchTag

Supports synchronization of scenarios on feature branches. See [Customization: Synchronizing scenarios from feature branches](../../features/push-features/customization-support-synchronizing-scenarios-from-a-branch.md) for details.

| Setting | Description | Default |
| ------- | ----------- | ------- |
| `branchTag/enabled` | Enables the customization. | `false` |
| `branchTag/prefix`  | The tag prefix to be used for linking scenarios that are updated on a branch. E.g. the prefix `tc.mybranch` will generate tags, like `@tc.mybranch:1234`. | mandatory |

### linkOnChange

Allows linking changed Test Cases to a work item or pull request, related to the change. See [Customization: Automatically link changed Test Cases](../../features/push-features/customization-automatically-link-changed-test-cases.md) for details.

| Setting | Description | Default |
| ------- | ----------- | ------- |
| `linkOnChange/enabled` | Enables the customization. | `false`   |
| `linkOnChange/links[]/targetId` | The ID of the work item or pull request to link the Test Case to. Placeholders, like `{env:ENVIRONMENT_VARIABLE}` can be used. | mandatory |
| `linkOnChange/links[]/targetType` | The type of the Azure DevOps work item the link refers to. It is verified at the time the link is established. | can link to any work item type |
| `linkOnChange/links[]/relationship` | Specify the relationship for the created link. E.g. specifying `Parent` means that the linked work item will be the parent of the test case work item. For linking Pull Requests it has to be set to `Pull Request` and `GitHub Pull Request` for GitHub Pull Requests (see details in our [guide](../../important-concepts/how-to-link-github-pull-requests.md#linking-github-pull-requests-when-the-test-case-changes)). | `Tests` |
| `linkOnChange/links[]/linkTemplate` | Specifies the HTTP link template of the related artifact (for `GitHub Pull Request` relationship). The link template can use the specified value using the `{id}` placeholder. | no template used |

### synchronizeLinkedArtifactTitles

Allows synchronizing linked artifact (work item) titles back to the local test case tags in `@story:123;This_is_the_story_title` format. See [Customization: Synchronize linked artifact titles](../../features/push-features/customization-sync-linked-artifact-titles.md) for details.

| Setting | Description | Default |
| ------- | ----------- | ------- |
| `synchronizeLinkedArtifactTitles/enabled` | Enables the customization. | `false`   |
| `synchronizeLinkedArtifactTitles/linkTagPrefixes` | Specifies the work item links to be considered. | mandatory |

### doNotSynchronizeTitle

Skips synchronizing the Test Case title field (`System.Title`). See [Customization: Do not synchronize title](../../features/push-features/customization-do-not-synchronize-title.md) for details.

| Setting | Description | Default |
| ------- | ----------- | ------- |
| `doNotSynchronizeTitle/enabled` | Enables the customization. | `false`   |

{% content-ref url="./" %}
[.](./)
{% endcontent-ref %}
