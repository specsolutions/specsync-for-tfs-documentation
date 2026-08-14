# Customization: Additional steps

The _Additional steps_ customization can be used to include additional steps in the step list based on tags.

{% hint style="info" %}
The _Additional steps_ customization described here is an [Enterprise feature](../../licensing.md).
{% endhint %}

Enable the customization in the `customizations/additionalSteps` section. The complete list of settings is available in the [customizations configuration reference](../../reference/configuration/configuration-customizations.md#additionalsteps).

The customization works similarly to the [Update Test Case fields feature](update-test-case-fields.md) and can be used to add additional steps to the step list of a Test Case based on tags of the source test. This is useful for test case sources supported via [SpecSync plugins](../plugin-list.md) (e.g. C# MsTest tests) where the plugin cannot detect steps itself.

The customization can contain multiple rules that are evaluated in the order they are defined. Each rule can contain a condition and a step to be added to the step list if the condition is met.

One rule is only evaluated once per Test Case. Therefore, you cannot have multiple tags that match to the same rule condition (e.g. multiple starting with `@given:...`), because only the first one will be processed. But you can add multiple steps with a single attribute, separating the steps within the attribute with new lines or {br} placeholders. 

The rules have an `isOutcomeStep` setting: `true` value tells SpecSync that that particular step is an "outcome" step. For outcome steps, you can configure SpecSync to use the "Expected result" field with setting synchronization/format/useExpectedResult to `true`.

The order of the added steps will be specified by the `rules` in the customization. The example below will add Given-When-Then steps regardless of the actual order of the tags.

Example configuration for adding additional steps based on tags like `@given:some_precondition_was_satisfied`:

{% code title="specsync.json" %}
```javascript
{
  ...
  "customizations": {
    "additionalSteps": {
      "enabled": true,
      "rules": [
        {
          "condition": "@given:*",
          "step": "Given {1:UnderscoreToSpace}"
        },
        {
          "condition": "@when:*",
          "step": "When {1:UnderscoreToSpace}"
        },
        {
          "condition": "@then:*",
          "step": "Then {1:UnderscoreToSpace}",
          "isOutcomeStep": true
        }
      ]
    }
  }
}
```
{% endcode %}
