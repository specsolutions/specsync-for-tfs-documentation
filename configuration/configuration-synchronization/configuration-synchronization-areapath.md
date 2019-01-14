# areaPath

This configuration section contains settings to configure how the test case _area path_ field should be updated.

Read more about the setting the _area path_ and _iteration path_ fields in the [Add new test cases to an Area or an Iteration](../../important-concepts/add-new-test-cases-to-an-area-or-an-iteration.md) concept description.

The following example shows the available options within this sub-section.

```text
{
  ...
  "synchronization": {
    ...
    "areaPath": {
      "mode": "setOnLink",
      "value": "\\MyArea"
    },
    ...
  },
  ...
}
```

## Settings

* `mode` -- Specifies how the area path of the test case should be updated. Available options: `notSet` and `setOnLink`. \(Default: `notSet`\)
  * `notSet`: the path is not set
  * `setOnLink`: set the path when the test case created and linked to the scenario, but not to update later on. 
* `value` -- The area path to set for test cases \(e.g. `\\MyArea`\). The project name prefix can be omitted.

_\[Back to the_ [`synchronization` _Configuration_](./)_\]_

_\[Back to the_ [_Configuration guide_](../)_\]_

