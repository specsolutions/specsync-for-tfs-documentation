# pull

This configuration section contains settings to configure the pull behavior.

Read more about the pull behavior in the [Two-way synchronization](../../../features/pull-features/two-way-synchronization.md) concept description.

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

| Setting | Description | Default |
| :--- | :--- | :--- |
| `enabled` | Enables changing the scenarios in the local repository based on the remote test cases. | `false` |
| `enableCreatingScenariosForNewTestCases` | Enables creating new scenarios from test cases that are not linked to any scenarios yet. | `false` |

{% page-ref page="./" %}

{% page-ref page="../" %}

