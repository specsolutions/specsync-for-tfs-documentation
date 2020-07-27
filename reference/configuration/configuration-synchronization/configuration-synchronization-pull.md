# pull

This configuration section contains settings to configure the pull behavior.

Read more about the pull behavior in the [Two-way synchronization](../../../important-concepts/two-way-synchronization.md) concept description.

_Note: The two-way synchronization is an_ [_Enterprise feature_](../../../licensing.md)_._

The following example shows the available options within this sub-section.

```javascript
{
  ...
  "synchronization": {
    ...
    "pull": {
      "enabled": true,
      "enableCreatingScenariosForNewTestCases": false
    },
    ...
  },
  ...
}
```

## Settings

* `enabled` -- Enables changing the scenarios in the local repository based on the remote test cases. \(Default: `false`\)
* `enableCreatingScenariosForNewTestCases` -- Enables creating new scenarios from test cases that are not linked to any scenarios yet. \(Default: `false`\)

_\[Back to the_ [`synchronization` _Configuration_](./)_\]_

_\[Back to the_ [_Configuration guide_](../)_\]_

