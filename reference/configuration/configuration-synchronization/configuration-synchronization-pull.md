# pull

This configuration section contains settings to configure the pull behavior.

Read more about the pull behavior in the [Two-way synchronization](../../../features/pull-features/two-way-synchronization.md) concept description.

{% hint style="info" %}
The two-way synchronization is an [Enterprise feature](../../../licensing.md).
{% endhint %}

The following example shows the available options within this sub-section.

```javascript
{
  ...
  "synchronization": {
    ...
    "pull": {
      "enabled": true,
      "enableCreatingScenariosForNewTestCases": false,
      "conflictResolution": "interactive"
    },
    ...
  },
  ...
}
```

## Settings

| Setting | Description | Default |
| ------- | ----------- | ------- |
| `enabled` | Enables changing the scenarios in the local repository based on the remote test cases. | `false` |
| `enableCreatingScenariosForNewTestCases` | Enables creating new scenarios from test cases that are not linked to any scenarios yet. | `false` |
| `conflictResolution` | Sets the conflict resolution method for pull. Possible values: `forceOverride`, `interactive`, `skip`, `error`, `ignore`. See [Synchronization conflict resolution](../../../features/common-synchronization-features/synchronization-conflict-resolution.md) for details. | `interactive` |



{% content-ref url="./" %}
[.](./)
{% endcontent-ref %}

{% content-ref url="../" %}
[..](../)
{% endcontent-ref %}
