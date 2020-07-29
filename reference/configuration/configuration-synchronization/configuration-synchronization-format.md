# format

This configuration section contains settings to configure the format of the synchronized test case.

Read more about the test case format options in the [Configuring the format of the synchronized test cases](../../../features/push-features/configuring-the-format-of-the-synchronized-test-cases.md) concept description.

The following example shows the available options within this sub-section.

```javascript
{
  ...
  "synchronization": {
    ...
    "format": {
      "useExpectedResult": false,
      "syncDataTableAsText": false,
      "prefixBackgroundSteps": true,
      "prefixTitle": true
    }
  },
  ...
}
```

## Settings

| Setting | Description | Default |
| :--- | :--- | :--- |
| `useExpectedResult` | If set to `true`, _Then_ steps will be synchronized to the _Expected result_ field of the test case steps. | `false` |
| syncDataTableAsText | If set to `true`, _Data Tables_ will be synchronized as plain text instead of HTML tables. | `false` |
| prefixBackgroundSteps | If set to `true`, _Background_ steps will be synchronized with the `Background:` prefix. | `true` |
| prefixTitle | If set to `true`, test case title will be synchronized with the `Scenario:` or `Scenario Outline:` prefix. | `true` |

{% page-ref page="./" %}

{% page-ref page="../" %}

