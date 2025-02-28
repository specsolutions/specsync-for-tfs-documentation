# Customization: Update custom Test Case fields on push

The _Custom field updates_ customization can be used to update arbitrary Test Case fields that are otherwise would not be changed by SpecSync.

{% hint style="warning" %}
The configuration setting `customizations/customFieldUpdates` has been replaced by `synchronization/fieldUpdates`. See [Update Test Case fields](update-test-case-fields.md) feature description for details.

This feature has been deprecated in v5 and will be removed in the next release. Please use the `specsync upgrade` command to upgrade the configuration to the replacement feature.
{% endhint %}

{% hint style="info" %}
The _Custom field updates_ customization described here is an [Enterprise feature](../../licensing.md).
{% endhint %}

In order to enable this customization, the `customizations/customFieldUpdates` section of the configuration has to be enabled. The complete reference of the configuration settings can be found in the [customizations configuration reference](../../reference/configuration/configuration-customizations.md#customfieldupdates).

The following example shows a basic configuration that initializes the _Description_ field to a message containing the feature name.

```javascript
{
  ...
 "customizations": {
    "customFieldUpdates": {
      "enabled": true,
      "updates": {
        "System.Description": "Synchronized from feature {feature-name}"
      }
    }
  }
  ...
}
```

The fields have to be identified using their reference name and *not their label*. E.g. the reference name for the _Description_ field is `System.Description`. The reference names of the built-in fields can be found in the [Azure DevOps documentation](https://docs.microsoft.com/en-us/azure/devops/boards/work-items/guidance/work-item-field?view=azure-devops). The reference name of the custom fields can be obtained from the Azure DevOps project administrator, but usually they follow the format `Custom.<your-field-name>` (e.g. `Custom.BusinessRule`).

{% hint style="warning" %}
Specifying an update for a field that is also updated by SpecSync (e.g. Title) causes unpredictable behavior.
{% endhint %}

The specified value is a template to be used to update the field. The template can contain placeholders listed in the [reference](../../reference/configuration/configuration-synchronization/configuration-synchronization-fieldupdates.md#update-placeholders).

The specified fields are only updated when the Test Case would otherwise change. If the test case is up-to-date, the custom fields are not going to be updated.

To initialize custom fields with a default value, use the [_Field defaults_ customization](customization-setting-test-case-fields-with-default-values.md).
