# Customization: Ignoring Test Case Tags

The _Ignore Test Case tags_ customization can be used to can be used to ignore test case tags, i.e. these tags are not going to be removed on push even if the scenario does not have a corresponding tag. This might be useful when the Test Cases need to be tagged in Azure DevOps with tags that are not replicated to the scenario in the feature file.

{% hint style="info" %}
The _Ignore Test Case tags_ customization described here is an [Enterprise feature](../../licensing.md).
{% endhint %}

In order to enable this customization, the `customizations/ignoreTestCaseTags` section of the configuration has to be enabled. The complete reference of the configuration settings can be found in the [customizations configuration reference](../../reference/configuration/configuration-customizations.md#ignoretestcasetags).

The following example shows a basic configuration that ignores the Test Case tag `mytag` and any Test Case tag starts with `ado-tag` (e.g. `ado-tag-important`).

```javascript
{
  ...
 "customizations": {
    "ignoreTestCaseTags": {
      "enabled": true,
      "tags": [ "mytag", "ado-tag*" ]
    }
  }
  ...
}
```

Multiple Test Case tag specifier can be listed in the `tags` setting. The tag specifier can be a tag (e.g. `mytag`) or a tag prefix with tailing wildcard (e.g. `ado-tag*` - ignores tags like `ado-tag-important`). The test case tags that match to any of the listed tag specifiers will be ignored by the synchronization.
