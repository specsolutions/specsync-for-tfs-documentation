# Customization: Ignore non-supported local tags

The _Ignore non-supported local tags_ customization is useful when the Azure DevOps settings does not allow creating new tags for normal users. With the customization you can specify those tags that are supported in the Azure DevOps project and SpecSync will ignore all other local (scenario) tags.

The _Ignore non-supported local tags_ customization described here is an [Enterprise feature](../../licensing.md).

In order to enable this customization, the `customizations/ignoreNotSupportedLocalTags` section of the configuration has to be enabled. The complete reference of the configuration settings can be found in the [customizations configuration reference](../../reference/configuration/configuration-customizations.md#ignoretestcasetags).

The following example shows a basic configuration that specifies that only the tags `myTag` and `otherTag` is supported in the Azure DevOps project.

```javascript
{
  ...
 "customizations": {
    "ignoreNotSupportedLocalTags": {
      "enabled": true,
      "supportedTags": [ "@myTag", "@otherTag" ]
    }
  }
  ...
}
```

Multiple Test Case tag specifier can be listed in the `supportedTags` setting. The tag specifier can be a tag (e.g. `@myTag`) or a tag prefix with tailing wildcard (e.g. `@ado-tag*` - specifies tags like `@ado-tag-important`as supported).
