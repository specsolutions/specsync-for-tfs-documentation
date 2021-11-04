# Customization: Publishing test results to multiple Test Suites

As described in the [Publishing test result files](publishing-test-result-files.md) page, the test results in Azure DevOps are always connected to a Test Suite. When a Test Case result is published to that Test Suite that result is not visible when you check the same Test Case in a different Test Suite.

The _Multi-suite publish_ customization for SpecSync allows publishing the same test results to multiple Test Suites (even into all Test Suite where the Test Case is included). Because of Azure DevOps limitations, these Test Suites must belong the the same Test Plan.

{% hint style="info" %}
The _Multi-suite publish _customization described here is an [Enterprise feature](../../licensing.md).
{% endhint %}

{% hint style="warning" %}
The Multi-suite publish customization is not supported in TFS 2017.
{% endhint %}

In order to enable the customization, the `customizations/multiSuitePublishTestResults` section of the configuration has to be enabled. The list of all possible settings can be found in the [customization configuration reference](../../reference/configuration/configuration-customizations.md). Since the Test Suites have to be in the same Test Plan, specifying the `testPlanId` setting is mandatory.

The following example shows a basic configuration.

{% code title="specsync.json" %}
```javascript
{
  ...
 "customizations": {
    "multiSuitePublishTestResults": {
      "enabled": true,
      "testPlanId": 567,
      "suites": [
        "SuiteA",
        "SuiteB",
      ]
    }
  }
  ...
}
```
{% endcode %}

The customization can be used in different ways:

| Goal                                                                                                                                                            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Publish test results to selected Test Suites within a Test Plan                                                                                                 | For that the selected Test Suites have to be listed in the `suites` setting. The list can contain Test Suite names or Test Suite IDs in `#1234` form. You can enable the `includeSubSuites` setting to include all suites of the listed ones.                                                                                                                                                                                                                                                                                                  |
| Publish to all Test Suites within the Test Plan                                                                                                                 | For that the `publishToAllSuites` flag has to be set to `true`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Publish to requirement-based Suites of the requirements [linked to the scenarios via tags](../common-synchronization-features/linking-work-items-with-tags.md). | <p>To be able to use this, the synchronization has to be configured to <a href="../common-synchronization-features/linking-work-items-with-tags.md">link Work Items using tags</a>.</p><p>To achieve this goal, the <code>publishToRequirementBasedTestSuites</code> flag has to be set to true. Optionally the work item links to be considered can be restricted using the <code>linkTagPrefixes</code> setting. The <code>publishToRequirementBasedTestSuites</code> setting can also be combined with the <code>suites</code> setting.</p> |

{% hint style="warning" %}
To be able to publish the same test results to multiple Test Suites, the created Test Run will contain duplicates of the test results. This makes the basic pass/fail statistics invalid.
{% endhint %}
