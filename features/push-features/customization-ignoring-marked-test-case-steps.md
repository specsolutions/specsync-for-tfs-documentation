# Customization: Ignoring marked Test Case steps

The _Ignore Test Case steps_ customization can be used to can be used to ignore (leave unchanged) test case steps with a specific prefix. This might be useful when additional information (e.g. about manual test execution) needs to be documented in the Test Case as steps.

{% hint style="info" %}
The _Ignore Test Case steps_ customization described here is an [Enterprise feature](../../licensing.md).
{% endhint %}

In order to enable this customization, the `customizations/ignoreTestCaseSteps` section of the configuration has to be enabled. The complete reference of the configuration settings can be found in the [customizations configuration reference](../../reference/configuration/configuration-customizations.md#ignoretestcasesteps).

The following example shows a basic configuration that ignores the Test Case steps prefixed with `COMMENT`.

```javascript
{
  ...
 "customizations": {
    "ignoreTestCaseSteps": {
      "enabled": true,
      "ignoredPrefixes": [ "COMMENT" ]
    }
  }
  ...
}
```

Multiple Test Case step prefix can be listed in the `ignoredPrefixes` setting. The prefixes are matched in a case insensitive way.

The ignores steps are moved to the top when the push command updates the Test Case.
