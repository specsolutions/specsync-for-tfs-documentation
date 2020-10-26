# Customization: Setting Test Case fields with default values

The _Field defaults_ customization can be used to initialize arbitrary Test Case fields that are otherwise would not be updated by SpecSync to a default value. This might be useful for custom Azure DevOps process templates.

{% hint style="info" %}
The _Field defaults_ customization described here is an [Enterprise feature](../../licensing.md).
{% endhint %}

In order to enable this customization, the `customizations/fieldDefaults` section of the configuration has to be enabled. The complete reference of the configuration settings can be found in the [customizations configuration reference](../../reference/configuration/configuration-customizations.md#fielddefaults).

The following example shows a basic configuration that initializes the field `MyCompany.MyCustomField` to `value1`.

```javascript
{
  ...
 "customizations": {
    "fieldDefaults": {
      "enabled": true,
      "defaultValues": {
        "MyCompany.MyCustomField": "value1"
      }
    }
  }
  ...
}
```

The fields have to be identified using their reference name and not their label. E.g. the reference name for the _Description_ field is `System.Description`. The reference names of the built-in fields can be found in the [Azure DevOps documentation](https://docs.microsoft.com/en-us/azure/devops/boards/work-items/guidance/work-item-field?view=azure-devops).

This customization uses the specified values only to _initialize_ the field when the Test Case is created. To update custom fields, use the [_Custom field updates_ customization](customization-update-custom-test-case-fields-on-push.md).

