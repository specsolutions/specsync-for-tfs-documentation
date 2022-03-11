# attachments

This configuration section contains settings to allow attaching files to Test Cases during synchronization using tags.

Read more about attaching files to Test Cases in the [Attach files to Test Cases using tags](../../../features/push-features/attach-files.md) page.

The following example shows the available options within this sub-section.

```javascript
{
  ...
  "synchronization": {
    ...
    "automation": {
      "attachments": true,
      "tagPrefixes": [
        "wireframe",
        "specification"
      ],
      "baseFolder": "resources/files"
    },
    ...
  },
  ...
}
```

## Settings

| Setting | Description | Default |
| ----------------------- | ----------------------- | ----------------------- |
| `enabled` | Specifies whether SpecSync should scan the `@attachment:...` tag or other specified tags to attach files to the Test Cases. | `false` |
| `tagPrefixes` | Specifies custom tag prefixes to be used for attachments. | `attachment` prefix is used |
| `baseFolder` | Specifies the base folder for the attached files. | uses the folder of feature file |

{% content-ref url="./" %}
[.](./)
{% endcontent-ref %}

{% content-ref url="../" %}
[..](../)
{% endcontent-ref %}
