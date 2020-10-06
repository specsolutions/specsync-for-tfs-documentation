# state

This configuration section contains settings to configure how the test case _state_ field should be updated. See [Setting Test Case State on change](../../../features/push-features/setting-test-case-state-on-change.md) for details.

The following example shows the available options within this sub-section.

```javascript
{
  ...
  "synchronization": {
    ...
    "state": {
      "setValueOnChangeTo": "Design"
    },
    ...
  },
  ...
}
```

## Settings

| Setting | Description | Default |
| :--- | :--- | :--- |
| `setValueOnChangeTo` | A state value \(e.g. `Design`\) to set test case state to when updating or creating a test case during synchronization. Useful for setting back `Ready` test cases to `Design` on change. | don't change test case state |

{% page-ref page="./" %}

{% page-ref page="../" %}

