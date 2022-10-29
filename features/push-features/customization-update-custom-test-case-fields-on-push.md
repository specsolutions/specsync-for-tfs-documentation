# Customization: Update custom Test Case fields on push

The _Custom field updates_ customization can be used to update arbitrary Test Case fields that are otherwise would not be changed by SpecSync.

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

The specified value is a template to be used to update the field. The template can contain placeholders listed below.

The specified fields are only updated when the Test Case would otherwise change. If the test case is up-to-date, the custom fields are not going to be updated.

To initialize custom fields with a default value, use the [_Field defaults_ customization](customization-setting-test-case-fields-with-default-values.md).

### Template placeholders

| Placeholder                       | Description                                                                                     |
| --------------------------------- | ----------------------------------------------------------------------------------------------- |
| `{feature-name}`                  | the name of the feature (specified in the feature file header)                                  |
| `{feature-description}`           | the description of the feature (the free-text block specified after the feature file header)    |
| `{feature-source}`                | The full feature file source text.                                                              |
| `{rule-name}`                     | The name of the rule that the scenario belongs to.                                              |
| `{scenario-name}`                 | the name of the scenario or scenario outline                                                    |
| `{scenario-description}`          | the description of the scenario or scenario outline                                             |
| `{scenario-source}`               | The full scenario source text.                                                                  |
| `{feature-file-name}`             | the file name of the feature file (without folder)                                              |
| `{feature-file-folder}`           | the folder of the feature file, relative to the project root                                    |
| `{feature-file-path}`             | the path (folder and file name) of the feature file, relative to the project root               |
| `{br}`                            | a new line                                                                                      |
| `{env:ENVIRONMENT_VARIABLE_NAME}` | The content of the environment variable specified (`ENVIRONMENT_VARIABLE_NAME` in this example) |
