# Customization: Reset Test Case state after change

The _Reset Test Case state_ customization can be used to reset Test Case state after change as a separate work item update based on tags. This is useful for example when the Test Case has to be marked as `Ready` if the scenario is tagged with `@ready`, but when work item updates are not allowed in `Ready` state.

{% hint style="info" %}
The _Reset Test Case state _customization described here is an [Enterprise feature](../../licensing.md).
{% endhint %}

In order to enable this customization, the `customizations/resetTestCaseState` section of the configuration has to be enabled. The complete reference of the configuration settings can be found in the [customizations configuration reference](../../reference/configuration/configuration-customizations.md#resettestcasestate).

The following example shows a basic configuration that resets the Test Case state to `Ready` if after change if the scenario is tagged with `@ready`.

```javascript
{
  ...
 "customizations": {
    "resetTestCaseState": {
      "enabled": true,
      "state": "Ready",
      "condition": "@ready"
    }
  }
  ...
}
```

If condition is not specified, the state is reset for all changed Test Case.
