# Customization: Add Test Cases to Suites

The _Add Test Cases to Suites_ customization can be used to include the synchronized Test Cases into various static Test Suites based on conditions.

{% hint style="info" %}
The _Add Test Cases to Suites_ customization described here is an [Enterprise feature](../../licensing.md).
{% endhint %}

{% hint style="info" %}
The _Add Test Cases to Suites_ customization is a preview feature for the planned support for synchronizing hierarchical test suites. Please help planning this feature by filling out our survey at https://forms.office.com/e/ZTda15AngN.
{% endhint %}

In order to enable this customization, the `customizations/addTestCasesToSuites` section of the configuration has to be enabled. The complete reference of the configuration settings can be found in the [customizations configuration reference](../../reference/configuration/configuration-customizations.md#addTestCasesToSuites).

The following example shows a basic configuration that adds all Test Cases to a Suite `All Test Cases` and all Test Cases that are synchronized from scenarios with `@important` tag but without `@external` tag to a Suite `Important Logic` in Test Plan #1234. In addition to these it adds all the Test Cases of scenarios within the `pricing` folder to a Suite named `Pricing`.

```javascript
{
  ...
 "customizations": {
    "addTestCasesToSuites": {
      "enabled": true,
      "testSuites": [
        {
          "name": "All Test Cases"
        },
        {
          "name": "Important Logic",
          "testPlanId": 1234,
          "condition": "@important and not @external"
        },
        {
          "name": "Pricing",
          "condition": "$sourceFile ~ pricing/"
        }
      ]
    }
  }
  ...
}
```

You can find more examples of the different local test case conditions that can be used in the [Local test case conditions documentation](../general-features/local-test-case-conditions.md).
