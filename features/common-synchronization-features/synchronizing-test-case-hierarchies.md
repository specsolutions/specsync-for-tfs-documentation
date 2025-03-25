# Synchronizing Test Case hierarchies using Test Suites

Besides [synchronizing local test case details](../push-features/pushing-scenario-changes-to-test-cases.md) and [test results](../test-result-publishing-features/publishing-test-result-files.md) to Azure DevOps Test Cases, SpecSync can also help you to organize the remote Test Cases into hierarchical structures. In Azure DevOps that means that it can create and maintain Test Suite hierarchies and include the Test Cases to these Test Suites according to the configured rules. The test results can also be published to such a synchronized hierarchy.

{% hint style="warning" %}
The Test Case hierarchy synchronization is only available to *SpecSync for Azure DevOps* currently. The feature will be available for *SpecSync for Jira* soon.
{% endhint %}

In SpecSync a Test Case *hierarchy* is a configured set of rules that defines a hierarchical structure for the Test Cases. The hierarchy defines several *nodes* (Test Suites in Azure DevOps), that might contain synchronized Test Cases. 

SpecSync will take care of
* Creating the hierarchy nodes
* Adding the Test Cases to the hierarchy nodes according to the configured rules
* Moving the Test Cases between hierarchy nodes when the rules conditions change (e.g. scenario is moved to another feature file)
* Removing the Test Case from the hierarchy node if it is detected to be removed (requires a configured [remote scope](TODO)).

Within one hierarchy each synchronized Test Case is included only once, but you can define multiple hierarchies. The hierarchy nodes might contain Test Cases that are not synchronized by SpecSync. These Test Cases have to be added to the hierarchy node manually, but SpecSync will not remove them during synchronization.

{% hint style="info" %}
Synchronizing to multiple hierarchies or hierarchies with more than 20 nodes requires an [Enterprise license](../../licensing.md).
{% endhint %}

You can configure the hierarchies based on different commonly used rule-sets by choosing a *hierarchy type*. For example the following configuration defines a hierarchy that is based on the folder structure of the local test case documents. I.e. every folder and sub-folder becomes a hierarchy node. In the example above, the root hierarchy node is mapped to the `Scenarios by Folder` Test Suite of the `My plan` Test Plan.

{% code title="specsync.json" %}
```json
{
  ...
  "hierarchies": [
    {
      "name": "folder-hierarchy",
      "type": "folders",
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

## Supported hierarchy types

The following table contains an overview of the supported hierarchy types. The sections below contain further details for the different types. The [](#common-hierarchy-settings) can be used for each hierarchy type.

| Type | Description |
| ---- | ----------- |
| `folders` | Defines the hierarchy based on the folders and sub-folders of the local test case documents (e.g. feature files). Each node will contain the local test cases (scenarios) of every document that is located directly in the related folder. See details [below](#folders-foldersandfiles-and-foldersanddocumentnames-hierarchy-types). |
| `foldersAndFiles` | Defines the hierarchy based on the folders, sub-folders and file names of the local test case documents (e.g. feature files). The file name extension is omitted. Each node will contain the test cases of the related local test case document. See details [below](#folders-foldersandfiles-and-foldersanddocumentnames-hierarchy-types). |
| `foldersAndDocumentNames` | Similar to `foldersAndFiles`, but for the leaf nodes, it uses the *name* of the local test case document instead of the file name when available. For example for feature files it uses the *feature name* (the name you specify after the `Feature:` prefix in the feature file). See details [below](#folders-foldersandfiles-and-foldersanddocumentnames-hierarchy-types). |
| `single` | Defines a hierarchy with a single node and all Test Cases that match to the optionally defined condition are included to that node. This hierarchy type can be seen as an direct alternative to the [Test Suite synchronization](group-synchronized-test-cases-to-a-test-suite.md) feature SpecSync provided earlier. |
| `levels` | Defines a hierarchy where the different levels correspond to a particular property of the test cases. E.g. the top level is defined by the area of the test: "Payment", "Inventory", etc; and the second level is defined by the test type: "UI", "API", etc. The Test Cases will be included in nodes, like "Payment / API". |
| `tag` | The hierarchy nodes are defined by tags with a prefix, e.g. `@suite:Payment/API`.|
| `custom` | Specifies a hierarchy by specifying each node with a condition that defines which test case should be included to that node. The test case will be included to the first node with a matching condition. |

## Common hierarchy settings

Regardless of the chosen hierarchy type there are some common settings that can be used.

* **`name`**: The name of the hierarchy is an identifier that can be used to refer to the hierarchy for other features (e.g. for publishing test results to that hierarchy). If no name is specified the name `default` is used. The name has to be unique among the defined hierarchies and it is mandatory when multiple hierarchies are specified.
* **`type`**: Specifies the type of the hierarchy. It is mandatory and has to be set to one of the available hierarchy types from the table above.
* **`condition`**: A [local test case condition](../general-features/local-test-case-conditions.md) to specify which test cases should be included to this hierarchy. By default all synchronized Test Cases is included.
* **`root`**: Specifies the root location in Azure DevOps, where the hierarchy should be mapped to. The root location specified here will be mapped to the root of the hierarchy. In Azure DevOps the root can be specified by specifying a Test Plan (`testPlan` setting) and either a Test Suite `name`, `path` or `id`. If only a Test Plan is specified, the root Test Suite of the plan will be used as the root. For most of the hierarchy types, specifying the `root` is mandatory.
* **`ignoreAdditionalNodes`**: By default SpecSync generates a warning if the hierarchy in Azure DevOps contains additional nodes (nodes that are not defined by the hierarchy). If such additional nodes are required, it is recommended to set this setting to `true` to avoid unnecessary warnings.

## `folders`, `foldersAndFiles` and `foldersAndDocumentNames` hierarchy types

The `folders`, `foldersAndFiles` and `foldersAndDocumentNames` hierarchy types define a hierarchy based on the source documents of the local test cases. 

The source documents are usually arranged into a logical folder and file structure that is also useful to maintain in Azure DevOps.

All three types uses the folders and sub-folders of the source documents (e.g. feature files) as a core structure. 

The default settings provide an 

