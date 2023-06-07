# Customization: Do not synchronize title

The _Do not synchronize title_ customization can be used if the Test Case title field (`System.Title`) has to be used for other purposes and SpecSync should not update it. 

The _Do not synchronize title_ customization described here is an [Enterprise feature](../../licensing.md).

In order to enable this customization, the `customizations/doNotSynchronizeTitle` section of the configuration has to be enabled. The complete reference of the configuration settings can be found in the [customizations configuration reference](../../reference/configuration/configuration-customizations.md#TODO).

The following example shows a basic configuration that enables this customization.

```javascript
{
  ...
 "customizations": {
    "doNotSynchronizeTitle": {
      "enabled": true
    }
  }
  ...
}
```

The title field of the Test Case will be set by SpecSync for new Test Cases even if the customization is enabled, because that is a mandatory field.

When the customization is enabled, the "pull" command will not be able to update the local test case title.
