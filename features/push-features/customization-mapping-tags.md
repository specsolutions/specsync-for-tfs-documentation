# Customization: Mapping tags

The _Tag text map transformation_ customization can be used to handle different notations between tags used in the feature file and tags in Azure DevOps \(e.g. how white spaces are handled\). 

{% hint style="info" %}
The _Tag text map transformation_ customization described here is an [Enterprise feature](../../licensing.md).
{% endhint %}

With the customization you can substitute characters or sub-strings in tags when synchronizing to Azure DevOps. E.g. underscores \(`_`\) in scenario tags can be represented with spaces in Test Case tags. The customization can even be used to substitute complete tags names as well.

In order to enable this customization, the `customizations/tagTextMapTransformation` section of the configuration has to be enabled. The complete reference of the configuration settings can be found in the [customizations configuration reference](../../reference/configuration/configuration-customizations.md#tagtextmaptransformation).

The following example shows a basic configuration that maps the underscore \(`_`\) character in Gherkin tags to space in Azure DevOps.

{% code title="specsync.json" %}
```javascript
{
  ...
 "customizations": {
    "tagTextMapTransformation": {
      "enabled": true,
      "textMap": {
        "_": " "
      }
    }
  }
  ...
}
```
{% endcode %}

