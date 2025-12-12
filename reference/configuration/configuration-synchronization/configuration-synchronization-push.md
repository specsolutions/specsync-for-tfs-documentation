# push

This configuration section contains settings to configure the push behavior.

Read more about the push behavior in the [Pushing scenario changes to Test Cases](../../../features/push-features/pushing-scenario-changes-to-test-cases.md) concept description.

The following example shows the available options within this sub-section.

```javascript
{
  ...
  "synchronization": {
    ...
    "push": {
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
| `conflictResolution` | Sets the conflict resolution method for push. Possible values: `overwrite`, `overwriteWithWarning`, `interactive`, `skipWithWarning`, `skip`, `error`. See [Synchronization conflict resolution](../../../features/common-synchronization-features/synchronization-conflict-resolution.md) for details. | `overwrite` |

{% content-ref url="./" %}
[.](./)
{% endcontent-ref %}

{% content-ref url="../" %}
[..](../)
{% endcontent-ref %}
