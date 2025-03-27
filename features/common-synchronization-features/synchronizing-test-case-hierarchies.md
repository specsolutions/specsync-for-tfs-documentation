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

You can configure the hierarchies based on different commonly used rule-sets by choosing a *hierarchy type*. For example the following configuration defines a hierarchy that is based on the folder structure of the local test case documents. I.e. every folder and sub-folder becomes a hierarchy node.

The following configuration defines a hierarchy named `folder-hierarchy` based on the folders and sub-folders of the files and includes all local test cases to the defined nodes that does not have an `@ignore` tag. In this example, the root hierarchy node is mapped to the `Scenarios by Folder` Test Suite of the `My plan` Test Plan.

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

This feature description describes the usage of this feature. You can also check the reference guide of the [`hierarchies` configuration setting](../../reference/configuration/configuration-hierarchies.md).

## Supported hierarchy types

The following table contains an overview of the supported hierarchy types. The sections below contain further details for the different types. The [common hierarchy settings](#common-hierarchy-settings) can be used for each hierarchy type.

| Type | Description |
| ---- | ----------- |
| `folders` | Defines the hierarchy based on the folders and sub-folders of the local test case documents (e.g. feature files). Each node will contain the local test cases (scenarios) of every document that is located directly in the related folder. See details [below](#the-folders-foldersandfiles-and-foldersanddocumentnames-hierarchy-types). |
| `foldersAndFiles` | Defines the hierarchy based on the folders, sub-folders and file names of the local test case documents (e.g. feature files). The file name extension is omitted. Each node will contain the test cases of the related local test case document. See details [below](#the-folders-foldersandfiles-and-foldersanddocumentnames-hierarchy-types). |
| `foldersAndDocumentNames` | Similar to `foldersAndFiles`, but for the leaf nodes, it uses the *name* of the local test case document instead of the file name when available. For example for feature files it uses the *feature name* (the name you specify after the `Feature:` prefix in the feature file). See details [below](#the-folders-foldersandfiles-and-foldersanddocumentnames-hierarchy-types). |
| `levels` | Defines a hierarchy where the different levels correspond to a particular property of the test cases. E.g. the top level is defined by the area of the test: "Payment", "Inventory", etc; and the second level is defined by the test type: "UI", "API", etc. The Test Cases will be included in nodes, like "Payment / API". See details [below](#the-levels-hierarchy-type). |
| `tag` | The hierarchy nodes are defined by tags with a prefix, e.g. `@suite:Payment/API`. See details [below](#the-tag-hierarchy-type).|
| `single` | Defines a hierarchy with a single node and all Test Cases that match to the optionally defined condition are included to that node. This hierarchy type can be seen as an direct alternative to the [Test Suite synchronization](group-synchronized-test-cases-to-a-test-suite.md) feature SpecSync provided earlier. See details [below](#the-single-hierarchy-type). |
| `custom` | Specifies a hierarchy by specifying each node with a condition that defines which test case should be included to that node. The test case will be included to the first node with a matching condition. See details [below](#the-custom-hierarchy-type). |

## Common hierarchy settings

Regardless of the chosen hierarchy type there are some common settings that can be used.

* **`name`**: The name of the hierarchy is an identifier that can be used to refer to the hierarchy for other features (e.g. for publishing test results to that hierarchy). If no name is specified the name `default` is used. The name has to be unique among the defined hierarchies and it is mandatory when multiple hierarchies are specified.
* **`type`**: Specifies the type of the hierarchy. It is mandatory and has to be set to one of the available hierarchy types from the table above.
* **`condition`**: A [local test case condition](../general-features/local-test-case-conditions.md) to specify which test cases should be included to this hierarchy. By default all synchronized Test Cases is included.
* **`root`**: Specifies the root location in Azure DevOps, where the hierarchy should be mapped to. The root location specified here will be mapped to the root of the hierarchy. In Azure DevOps the root can be specified by specifying a Test Plan (`testPlan` setting) and either a Test Suite `name`, `path` or `id`. If only a Test Plan is specified, the root Test Suite of the plan will be used as the root. For most of the hierarchy types, specifying the `root` is mandatory.
* **`ignoreAdditionalNodes`**: By default SpecSync generates a warning if the hierarchy in Azure DevOps contains additional nodes (nodes that are not defined by the hierarchy). If such additional nodes are required, it is recommended to set this setting to `true` to avoid unnecessary warnings.
* **`disableUnderscoreTransformation`**: The `_` character in the matched node names are automatically transformed to space by default. This behavior can be disabled by setting the `disableUnderscoreTransformation` hierarchy setting to `true`. This setting can be used for `levels` and `tag` hierarchy types.


## The `folders`, `foldersAndFiles` and `foldersAndDocumentNames` hierarchy types

The `folders`, `foldersAndFiles` and `foldersAndDocumentNames` hierarchy types define a hierarchy based on the source documents of the local test cases. 

The source documents are usually arranged into a logical folder and file structure that is also useful to maintain in Azure DevOps.

All three types uses the folders and sub-folders of the source documents (e.g. feature files) as a core structure, but `foldersAndFiles` and `foldersAndDocumentNames` also defines nodes based on the source documents (e.g. feature files) as well.

Let's assume the following local test case structure. The SpecSync configuration root folder is the folder that contains the `specsync.json` configuration file. 

{% hint style="info" %}
If the files are not directly in the SpecSync configuration root folder, but in a sub-folder, you can consider using the `skipFolderPrefix` setting, see details [below](#skipping-folder-prefix).
{% endhint %}

```text
<SpecSync configuration root folder>
├ Authentication.feature
│  └ Scenario: User authenticates successfully with token (#1)
├ Payments/
│  ├ CardPayments.feature
│  │  ├ Scenario: Card payment is successful (#2)
│  │  └ Scenario: Card payment was cancelled (#3)
│  └ BankTransferPayments.feature
│     └ Scenario: Bank payment instructions are sent (#4)
└ Inventory/
   ├ MaintainInventory.feature
   │  ├ Scenario: A new product is added to the Inventory (#5)
   │  └ Scenario: A product is removed from the Inventory (#6)
   ├ DisplayInventory.feature
   │  └ Scenario: The details of a product is displayed (#7)
   └ Reporting/
      └ InventoryReports.feature
         └ Scenario: Inventory day close report is generated (#8)
```

Choosing the `folders` hierarchy type would generate the following hierarchy structure from the local test case structure above.

```text
<root>: Contains #1
├ Payments: Contains #2, #3, #4
└ Inventory: Contains #5, #6, #7
   └ Reporting: Contains #8
```

The `<root>` node can be mapped to a root location in Azure DevOps, see [common hierarchy settings](#common-hierarchy-settings) for details. In Azure DevOps the root location can be a Test Suite in a Test Plan (or the root Test Suite of the Test Plan).

Choosing the `foldersAndFiles` hierarchy type would define nodes from the files as well.
```text
<root>: Empty
├ Authentication: Contains #1
├ Payments: Empty
│  ├ CardPayments: Contains #2, #3
│  └ BankTransferPayments: Contains #4
└ Inventory: Empty
   ├ MaintainInventory: Contains #5, #6
   ├ DisplayInventory: Contains #7
   └ Reporting: Empty
      └ InventoryReports: Contains #8
```


Choosing the `foldersAndDocumentNames` hierarchy type almost identical to `foldersAndFiles` except that it uses the *name* of the local test case document instead of the file name when available. For example for feature files it uses the *feature name* (the name you specify after the `Feature:` prefix in the feature file).

Assuming the feature names of the example files are the same of the file names except of spaces, the hierarchy would look like the following.

```text
<root>: Empty
├ Authentication: Contains #1
├ Payments: Empty
│  ├ Card Payments: Contains #2, #3
│  └ Bank Transfer Payments: Contains #4
└ Inventory: Empty
   ├ Maintain Inventory: Contains #5, #6
   ├ Display Inventory: Contains #7
   └ Reporting: Empty
      └ Inventory Reports: Contains #8
```

### Skipping folder prefix

In some cases the source documents are in a sub-folder of the SpecSync configuration root folder and you don't want to represent those folders in the hierarchy. Let's consider the following structure.

```text
<SpecSync configuration root folder>
└ src/
   └ features/
      ├ Authentication.feature
      └ Inventory/
        ├ MaintainInventory.feature
        ├ DisplayInventory.feature
        └ Reporting/
            └ InventoryReports.feature
```

By default the `src/features` folders would become part of the hierarchy. For example with `folders` type, it would generate the following structure.


```text
<root>
└ src
   └ features
      └ Inventory
        └ Reporting
```

To avoid generating hierarchy nodes from `src/features`, you can use the `skipFolderPrefix` setting:

{% code title="specsync.json" %}
```json
{
  ...
  "hierarchies": [
    {
      "type": "folders",
      "skipFolderPrefix": "src/features"
    }
  ],
  ...
}
```
{% endcode %}

As a result, the following hierarchy is generated.

```text
<root>
└ Inventory
  └ Reporting
```

The `skipFolderPrefix` setting can be used for `folders`, `foldersAndFiles` and `foldersAndDocumentNames` hierarchy types.

## The `levels` hierarchy type

The `levels` hierarchy type defines a hierarchy where the different levels correspond to a particular property or categorization of the test cases. This is useful when the levels of the desired structure is defined according to some aspects. 

Let's assume we would like to build a hierarchy with three levels:

* level 1: area (e.g. "Payments")
* level 2: component (e.g. "FrontEnd")
* level 3: test category (e.g. "Regression")

With this specification the Test Cases would be included to hierarchy nodes, like "Payments / FrontEnd / Regression". 

Consider the following feature file.

{% code title="Features/Payments/CardPayments.feature" %}
```gherkin
Feature: Card Payments

@component:FrontEnd
Rule: Card payment can be initiated from the UI

  @smoke
  Scenario: Card payment is started (#1)
  [...]
  
  @regression
  Scenario: Card payment is successful (#2)
  [...]

Rule: Card payment is logged

  @component:BackEnd
  @regression
  Scenario: Card payment is added to transaction log (#3)
  [...]
```
{% endcode %}

The folder of the feature file (`Payments`), the tags of the scenarios and the tags "inherited" from the `Feature` and `Rule` headers define the aspects of the test cases that we would like to use:

* Scenario #1: Card payment is started
  * Area: Payments
  * Component: FrontEnd
  * Category: Smoke
* Scenario #2: Card payment is successful
  * Area: Payments
  * Component: FrontEnd
  * Category: Regression
* Scenario #3: Card payment is added to transaction log
  * Area: Payments
  * Component: BackEnd
  * Category: Regression

{% hint style="info" %}
In some cases a particular aspect cannot be detected (e.g. the scenario has neither `@regression` nor `@smoke` tag). You can configure how SpecSync should handle this case with the `onNotMatching` and the `nameForNotMatching` level settings. See details [below](#handling-non-matching-levels).
{% endhint %}

The levels can be specified with conditions similar to the ones that are used for [updating Test Case fields](../push-features/update-test-case-fields.md).

The following hierarchy configuration can be used to define the hierarchy with the "area", "component" and "test category" levels.

{% code title="specsync.json" %}
```json
{
  ...
  "hierarchies": [
    {
      "type": "levels",
      "levels": [
        { // level 1: area
          "condition": "$sourceFile ~ Features/*/" // takes the folder below Features/
        },
        { // level 2: component
          "condition": "@component:*" // takes the component from tag
        },
        { // level 3: test category
          "conditionalName": {
            "@smoke": "Smoke",
            "@regression": "Regression"
          }
        }
      ]
    }
  ],
  ...
}
```
{% endcode %}

Synchronizing the feature file above would generate the following hierarchy.

```text
<root>: Empty
└ Payments: Empty
   ├ FrontEnd: Empty
   │  ├ Smoke: Contains #1
   │  └ Regression: Contains #2
   └ BackEnd: Empty
      └ Regression: Contains #3
```

### Customize node name

When a level is defined with a wildcard match the hierarchy node name will use the value that has matched to the wildcard. The default behavior can be changed by specifying the `name` level setting.

Let's say we would like to define the second level as `FrontEnd Category` and `BackEnd Category` instead of `FrontEnd` and `BackEnd`. This can be achieved by the following configuration.

{% code title="specsync.json" %}
```json
{
  ...
  "hierarchies": [
    {
      "type": "levels",
      "levels": [
        { // level 1: area
          [...]
        },
        { // level 2: component
          "condition": "@component:*",
          "name": "{1} Category"
        },
        { // level 3: test category
          [...]
        }
      ]
    }
  ],
  ...
}
```
{% endcode %}

In the `name` level setting, the placeholder `{1}` refers to the first wildcard, `{2}` to the second wildcard (if present), and so on. 

With this configuration, the generated hierarchy would become:

```text
<root>
└ Payments
   ├ FrontEnd Category
   │  ├ Smoke
   │  └ Regression
   └ BackEnd Category
      └ Regression
```

{% hint style="info" %}
The matched level name may contain multiple hierarchy path nodes separated by `/`. This can be used for special cases, for example a tag `@frontEndSmoke` could be mapped to the hierarchy node path `FrontEnd/Smoke`.
{% endhint %}


{% hint style="info" %}
The `_` character in the matched wildcards are automatically transformed to space by default. So for example the tag `@category:FrontEnd_Category` using the condition `@category:*` would define a node name `FrontEnd Category`. This behavior can be disabled by setting the `disableUnderscoreTransformation` hierarchy setting to `true`.
{% endhint %}


### Handling non-matching levels

Ideally the value for each level can be detected based on the source path and the tags. In some cases some cases the scenarios are classified to a "default" category. For example the team may decide that if the scenario has no `@component:xxx` tag, it should belong to the "General" component. They might also decide to just include these Test Cases to the parent hierarchy node.

SpecSync provides multiple options to handle this situation by specifying the `onNotMatching` level setting. For example the following configuration uses a "General" component if the `@component:xxx` tag is missing.


{% code title="specsync.json" %}
```json
{
  ...
  "hierarchies": [
    {
      "type": "levels",
      "levels": [
        { // level 1: area
          [...]
        },
        { // level 2: component
          "condition": "@component:*",
          "onNotMatching": "customName",
          "nameForNotMatching": "General"
        },
        { // level 3: test category
          [...]
        }
      ]
    }
  ],
  ...
}
```
{% endcode %}

The following table contains the possible values for the `onNotMatching` level setting. The default is `ignore` for top level and `skipLevel` for the other levels.

| Value | Description |
| ----- | ----------- |
| `ignore` | The Test Case is ignored and will not be included to this hierarchy. In the example above, if `@component:xxx` tag is missing the Test Case would not be included in the hierarchy. |
| `skipLevel` | The non-matching level is skipped, so all levels below would be promoted by one. In the example above, if `@component:xxx` tag is missing the Test Case would be included in a node like "Payments / Smoke". |
| `includeToParent` | The Test Case is included to the hierarchy node of the previous (parent) level node but the remaining levels are not processed. In the example above, if `@component:xxx` tag is missing the Test Case would be included in the "Payments" node, regardless of whether it has `@smoke` tag. |
| `customName` | A custom name is used instead that has to be specified using the `nameForNotMatching` level setting. In the example above, if `@component:xxx` tag is missing a "General" component level can be used instead. |

## The `tag` hierarchy type

The `tag` hierarchy type defines the hierarchy based on the hierarchy node paths specified using tags of a specified tag prefix.

Consider the following feature file.

{% code title="CardPayments.feature" %}
```gherkin
Feature: Card Payments

Rule: Card payment can be initiated from the UI

  @suite:Payments/FrontEnd/Smoke
  Scenario: Card payment is started (#1)
  [...]
  
  @suite:Payments/FrontEnd/Regression
  Scenario: Card payment is successful (#2)
  [...]

Rule: Card payment is logged

  @suite:Payments/BackEnd/Regression
  Scenario: Card payment is added to transaction log (#3)
  [...]
```
{% endcode %}

The scenarios in the example above are tagged with a `@suite:xxx` tag that specifies the hierarchy node path (Test Suite path in Azure DevOps).

The following hierarchy configuration can be used to define this hierarchy.

{% code title="specsync.json" %}
```json
{
  ...
  "hierarchies": [
    {
      "type": "tag",
      "tagPrefix": "suite"
    }
  ],
  ...
}
```
{% endcode %}

Synchronizing the feature file above with this configuration would generate the following hierarchy.

```text
<root>: Empty
└ Payments: Empty
   ├ FrontEnd: Empty
   │  ├ Smoke: Contains #1
   │  └ Regression: Contains #2
   └ BackEnd: Empty
      └ Regression: Contains #3
```

{% hint style="info" %}
SpecSync uses colon `:` as a default separator for values specified with tag prefixes. The allowed separators can be configured using the `synchronization/tagPrefixSeparators` configuration setting. For example setting this to `[':', '=']` would allow defining the path with both `@suite:Payments/FrontEnd/Smoke` and `@suite=Payments/FrontEnd/Smoke`.
{% endhint %}

{% hint style="info" %}
The `/` character in the tag name is used to define the hierarchy structure. If a node name needs to have the `/` character, it has to be wrapped with quotes (`'` or `"`). For example the tag `@suite:"A/C"/Smoke` defines a two-level hierarchy, with the first level named `A/C` and the second level as `Smoke`.
{% endhint %}

{% hint style="info" %}
The `_` character in the tag name is automatically transformed to space by default. So for example the tag `@suite:Payments/FrontEnd_Category` would define a node with the second level as `FrontEnd Category`. This behavior can be disabled by setting the `disableUnderscoreTransformation` hierarchy setting to `true`.
{% endhint %}

## The `single` hierarchy type

The `single` hierarchy type defines a hierarchy with a single node and all Test Cases that match to the optionally defined condition are included to that node.

{% hint style="info" %}
This hierarchy type can be seen as an direct alternative to the [Test Suite synchronization](group-synchronized-test-cases-to-a-test-suite.md) feature SpecSync provided earlier.
{% endhint %}

The following configuration defines a hierarchy with a single "BDD Scenarios" node(in Test Plan "My Plan") and includes all synchronized test cases that does not have an `@ignore` tag.

{% code title="specsync.json" %}
```json
{
  ...
  "hierarchies": [
    {
      "type": "single",
      "condition": "not @ignore",
      "node": {
        "name": "BDD Scenarios",
        "testPlan": "My plan"
      }
    }
  ],
  ...
}
```
{% endcode %}

{% hint style="info" %}
Specifying the root of the hierarchy using the `root` setting is not required, but if specified the settings in `node` overwrite the settings in `root`.
{% endhint %}

{% hint style="info" %}
If condition is not specified, all Test Cases are included into the hierarchy node.
{% endhint %}

## The `custom` hierarchy type

The `custom` hierarchy type defines a hierarchy by configuring each node separately. For each node you can specify the node name or path and a condition that specifies which Test Cases should be included in that node. The test case will be included to the first node with a matching condition.

{% hint style="info" %}
This hierarchy type can be seen as an direct alternative to the [Add Test Cases to Suites](../push-features/customization-add-test-cases-to-suites.md) customization SpecSync provided earlier.
{% endhint %}

Consider the following feature file.

{% code title="Features/Payments/CardPayments.feature" %}
```gherkin
Feature: Card Payments

Rule: Card payment can be initiated from the UI

  @smoke
  Scenario: Card payment is started (#1)
  [...]
  
  @regression
  Scenario: Card payment is successful (#2)
  [...]

Rule: Card payment is logged

  @regression
  Scenario: Card payment is added to transaction log (#3)
  [...]
```
{% endcode %}

The following configuration defines a hierarchy similar to the example we defined using the [`levels` type](#the-levels-hierarchy-type).


{% code title="specsync.json" %}
```json
{
  ...
  "hierarchies": [
    {
      "type": "custom",
      "nodes": [
        {
          "path": "Payments/Smoke",
          "condition": "$sourceFile ~ Features/Payments/ and @smoke"
        },
        {
          "path": "Payments/Regression",
          "condition": "$sourceFile ~ Features/Payments/ and @regression"
        },
        {
          "name: "General"
        }
      ]
    }
  ],
  ...
}
```
{% endcode %}

{% hint style="info" %}
The node configurations within the `nodes` setting cannot be nested, but you can specify the nodes using `path` instead of `name` to build up a nested node hierarchy.
{% endhint %}

Synchronizing the feature file above with this configuration would generate the following hierarchy.

```text
<root>: Empty
└ Payments: Empty
   ├ Smoke: Contains #1
   └ Regression: Contains #2, #3
```

The third configured node ("Generic") does not have a condition, so it will include all Test Cases that have not been included by the conditions of the prior nodes. This means that if there is an additional scenario that is not within the `Features/Payments/` folder or does not have neither a `@smoke` nor a `@regression` tag, it will be included here.