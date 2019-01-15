# customizations

This configuration section contains settings for configuring customizations.

_Note: The customizations described here are_ [_Enterprise features_](../licensing.md)_._

The following example shows the available options within this section.

```javascript
{
  ...
 "customizations": {
    "branchTag": {
      "enabled": true,
      "prefix": "tc.mybranch"
    },
    "fieldDefaults": {
      "enabled": true,
      "defaultValues": {
        "MyCompany.MyCustomField": "Default 1",
        "MyCompany.OtherCustomField": "Default 2"
      }
    },
    "ignoreTestCaseSteps": {
      "enabled": true,
      "ignoredPrefixes": [ "COMMENT" ]
    },
    "customFieldUpdates": {
      "enabled": true,
      "updates": {
        "System.Description": "Syncronized from feature {feature-name}{br}{feature-description}"
      }
    }
  }
  ...
}
```

## Settings

* `branchTag` -- Supports synchronization of scenarios on a feature branch.
  * `branchTag/enabled` -- Enables the customization. \(Default: `false`\)
  * `branchTag/prefix` -- The tag prefix to be used for linking scenarios that are updated on a branch. E.g. the prefix `tc.mybranch` will generate tags, like `@tc.mybranch:1234`.
* `fieldDefaults` -- Enables setting default values to test case fields. Useful for custom Azure DevOps process templates.
  * `fieldDefaults/enabled` -- Enables the customization. \(Default: `false`\)
  * `fieldDefaults/defaultValues` -- A list of key-value pair, where the key is the canonical name of the field to be updated \(e.g. `System.Description`\) and the value is the default value to be used when the test case is created. 
* `ignoreTestCaseSteps` -- Can ignore \(leave unchanged\) test case steps with a specific prefix.
  * `ignoreTestCaseSteps/enabled` -- Enables the customization. \(Default: `false`\)
  * `ignoreTestCaseSteps/ignoredPrefixes` -- An array of prefixes. The test case steps that start with any of the listed prefixes \(case-insensitive\) will be ignored by the synchronization.
* `customFieldUpdates` -- Enables updating test case fields that are normally not changed by SpecSync.
  * `customFieldUpdates/enabled` -- Enables the customization. \(Default: `false`\)
  * `customFieldUpdates/updates` -- A list of key-value pair, where the key is the canonical name of the field to be updated \(e.g. `System.Description`\) and the value is the template to be used to update the field. The template can contain the following placeholders
    * `{feature-name}` -- the name of the feature \(specified in the feature file header\)
    * `{feature-description}` -- the description of the feature \(the free-text block specified after the feature file header\)
    * `{scenario-name}` -- the name of the scenario or scenario outline
    * `{scenario-description}` -- the description of the scenario or scenario outline 
    * `{feature-file-name}` -- the file name of the feature file \(without folder\)
    * `{feature-file-folder}` -- the folder of the feature file, relative to the project root
    * `{feature-file-path}` -- the path \(folder and file name\) of the feature file, relative to the project root

_\[Back to the_ [_Configuration guide_](./)_\]_

