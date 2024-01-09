# Mark Test Cases as Automated

By default the push command creates the Test Cases as `Not Automated`. To be able to synchronize automated Test Cases (for all or for selected scenarios), you can configure SpecSync for this.&#x20;

Marking the Test Cases as automated can be used to document the kind of the test. For using certain features of Azure DevOps (e.g. [Test Suite based execution](../test-result-publishing-features/support-for-azure-devops-test-plan-test-suite-based-test-execution.md)), marking the Test Cases as Automated is mandatory.

You can enable marking Test Cases as automated by setting the [`synchronization/automation/enabled` configuration setting](../../reference/configuration/configuration-synchronization/configuration-synchronization-automation.md) to `true`.

{% hint style="info" %}
When the automation synchronization is not enabled, SpecSync resets the automation status to `Not Automated` when the Test Case is updated. In order to control the automation status with another tool or using the [Update Test Case fields](update-test-case-fields.md#update-automation-fields) feature, you have to set the `toolSettings/doNotSynchronizeAutomationUnlessEnabled` configuration setting is set to `true` and the `synchronization/automation/enabled` configuration setting to `false` or leave it unset.
{% endhint %}

Optionally you can also specify the value to be used for the "Automated test type" field of the Test Case using the `synchronization/automation/automatedTestType` setting. (By default the field is set to `SpecFlow` for SpecFlow projects and `Gherkin` for other projects.)

{% hint style="info" %}
After changing the automation configuration, you need to perform the push command with an additional `--force` setting. Using the `--force` setting is only required once.
{% endhint %}

{% code title="specsync.json" %}
```javascript
{
  ...
  "synchronization": {
    ...
    "automation": {
      "enabled": true
    },
    ...
  },
  ...
}
```
{% endcode %}

{% hint style="warning" %}
In SpecSync v3.0 or earlier, the `synchronization/automation/testExecutionStrategy` had to be also specified when automation was enabled. From v3.1 this is not necessary and recommended to leave it unset, unless you want to use the legacy [test-suite based execution](../../features/test-result-publishing-features/support-for-azure-devops-test-plan-test-suite-based-test-execution.md) method.
{% endhint %}

## Marking only certain Test Cases as automated

In some projects the feature files contain non-automated scenarios as well. These scenarios are usually marked with specific tags, like `@manual` or `@ignore` or the automated ones are marked with a specific tag, like `@automated`.&#x20;

In order to mark only the Test Cases for selected scenarios as automated, you can set a [local test case condition](../general-features/local-test-case-conditions.md) in the `synchronization/automation/condition` configuration setting as shown below.

{% code title="specsync.json" %}
```javascript
{
  ...
  "synchronization": {
    ...
    "automation": {
      "enabled": true,
      "condition": "not @manual"
    },
    ...
  },
  ...
}
```
{% endcode %}

## Update the Test Case automation fields to custom values

The methods described above would update the Test Case automation fields to generally usable values. If your project configuration requires specific custom values to be set for these fields, you can update the fields individually using the `synchronization/fieldUpdates` configuration section that allows you to set values conditionally or decide if you want to set the values only when the Test Case is created (set "default" values) or on each update.

A detailed description of the steps required to configure these for the automation fields can be found in the [Update automation fields section](update-test-case-fields.md#update-automation-fields) of the "Update Test Case fields" feature description.
