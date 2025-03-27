# hierarchies

Specifies Test Case hierarchies to construct from the synchronized Test Cases.

{% hint style="warning" %}
The Test Case hierarchy synchronization is only available to *SpecSync for Azure DevOps* currently. The feature will be available for *SpecSync for Jira* soon.
{% endhint %}

To read more about synchronizing Test Case hierarchies and further examples see the [Synchronizing Test Case hierarchies using Test Suites](../../features/common-synchronization-features/synchronizing-test-case-hierarchies.md) page.

{% hint style="info" %}
Synchronizing to multiple hierarchies or hierarchies with more than 20 nodes requires an [Enterprise license](../../licensing.md).
{% endhint %}

The following example shows the most common options within this section.

{% code title="specsync.json" %}
```json
{
  ...
  "hierarchies": [
    {
      "name": "folder-hierarchy",
      "type": "folders",
      "condition": "not @ignore",
      "root": {
        "testPlan": "My plan",
        "name": "Scenarios by Folder"
      }
    }
  ],
  ...
}
```
{% endcode %}

## Settings

| Setting | Description | Default |
| ------- | ----------- | ------- |
|`name` | The name of the hierarchy is an identifier that can be used to refer to the hierarchy for other features (e.g. for publishing test results to that hierarchy). The name has to be unique among the defined hierarchies and it is mandatory when multiple hierarchies are specified. | `default` |
| `type` | Specifies the type of the hierarchy. It has to be set to one of the available [hierarchy types](../../features/common-synchronization-features/synchronizing-test-case-hierarchies.md#supported-hierarchy-types). | mandatory |
| `condition` | A [local test case condition](../../features/general-features/local-test-case-conditions.md) to specify which test cases should be included to this hierarchy. | all synchronized Test Cases is included |
| `root` | Specifies the root location in Azure DevOps, where the hierarchy should be mapped to. The root location specified here will be mapped to the root of the hierarchy. In Azure DevOps the root can be specified by specifying a Test Plan (`testPlan` setting) and either a Test Suite `name`, `path` or `id`. If only a Test Plan is specified, the root Test Suite of the plan will be used as the root. | For most of the hierarchy types it is mandatory. |
| `ignoreAdditionalNodes` | By default SpecSync generates a warning if the hierarchy in Azure DevOps contains additional nodes (nodes that are not defined by the hierarchy). If such additional nodes are required, it is recommended to set this setting to `true` to avoid unnecessary warnings. | `false` |
| `disableUnderscoreTransformation` | The `_` character in the matched node names are automatically transformed to space by default. This behavior can be disabled by setting the `disableUnderscoreTransformation` hierarchy setting to `true`. This setting can be used for `levels` and `tag` hierarchy types. | `false` |
| `skipFolderPrefix` | For type `folders`, `foldersAndFiles` or `foldersAndDocumentNames`: A project-relative folder prefix to skip when constructing the hierarchy node path from the folder structure (e.g. `src/Features`). | the full project-relative path is used |
| `levels` | For type `levels`: The level specifications. The items can contain settings: `condition`, `name`, `conditionalName`, `onNotMatching`, `nameForNotMatching`. See the [`levels` type description](../../features/common-synchronization-features/synchronizing-test-case-hierarchies.md#the-levels-hierarchy-type) for details. | mandatory for `levels` |
| `tagPrefix` | For type `tag`: Specifies the tag prefix that specifies the hierarchy path (e.g. 'suite'). The configured tag prefix separators (by default ':') can be used with tags, e.g. `@suite:Pricing/Automated`. See the [`tag` type description](../../features/common-synchronization-features/synchronizing-test-case-hierarchies.md#the-tag-hierarchy-type) for details. | mandatory for `tag` |
| `node` | For type `single`: The single node of the hierarchy. It can contain settings: `condition`, `name`, `path`, `id`, `testPlan`. See the [`single` type description](../../features/common-synchronization-features/synchronizing-test-case-hierarchies.md#the-single-hierarchy-type) for details. | mandatory for `single` |
| `nodes` | For type `custom`: The nodes of the hierarchy. The items can contain settings: `condition`, `name`, `path`, `id`, `testPlan`. See the [`custom` type description](../../features/common-synchronization-features/synchronizing-test-case-hierarchies.md#the-custom-hierarchy-type) for details. | mandatory for `custom` |

{% page-ref page="./" %}
