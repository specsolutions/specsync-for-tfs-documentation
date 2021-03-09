# state

This configuration section contains settings to configure how the test case _state_ field should be updated. See [Setting Test Case State on change](../../../features/push-features/setting-test-case-state-on-change.md) for details.

The following example shows the available options within this sub-section.

```javascript
{
  ...
  "synchronization": {
    ...
    "state": {
      "setValueOnChangeTo": "Design",
      "condition": "@ready"
    },
    ...
  },
  ...
}
```

## Settings

| Setting | Description | Default |
| :--- | :--- | :--- |
| `setValueOnChangeTo` | A state value \(e.g. `Design`\) to set test case state to when updating or creating a test case during synchronization. Useful for setting back test cases to state `Design` on change. The possible allowed values might depend on the process template your project uses. | don't change test case state |
| condition | A [tag expression](http://speclink.me/tagexpressions) of scenarios that should be included for state change \(e.g. `@inprogress` , `not @ready`\). | all scenarios included for state change |

{% page-ref page="./" %}

{% page-ref page="../" %}

