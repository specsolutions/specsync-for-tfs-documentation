# Mark Test Cases as Automated

By default the push command creates the Test Cases as `Not Automated`. To be able to synchronize automated Test Cases \(for all or for selected scenarios\), you can configure SpecSync for this. 

Marking the Test Cases as automated can be used to document the kind of the test. For using certain features of Azure DevOps \(e.g. [Test Suite based execution](../test-result-publishing-features/support-for-azure-devops-test-plan-test-suite-based-test-execution.md)\), marking the Test Cases as Automated is mandatory.

You can enable marking Test Cases as automated by setting the [`synchronization/automation/enabled` configuration setting](../../reference/configuration/configuration-synchronization/configuration-synchronization-automation.md) to `true`.

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
In SpecSync v3.0 or earlier, the `synchronization/automation/testExecutionStrategy` had to be also specified when automation was enabled. From v3.1, the `assemblyBasedExecution` is used as a default - it is recommended to use this value for older versions as well.
{% endhint %}

## Marking only certain Test Cases as automated

In some projects the feature files contain non-automated scenarios as well. These scenarios are usually marked with specific tags, like @manual or @ignore. 

In order to not mark the Test Cases for these scenarios as automated, you can set a [tag expression](http://speclink.me/tagexpressions) in the `synchronization/automation/skipForTags` configuration setting as shown below.

{% code title="specsync.json" %}
```javascript
{
  ...
  "synchronization": {
    ...
    "automation": {
      "enabled": true,
      "skipForTags": "@manual or @ignore"
    },
    ...
  },
  ...
}
```
{% endcode %}

{% hint style="info" %}
To only mark some tagged scenarios as automated, you can use the `not` operator in the tag expression. E.g. to synchronize automated Test Cases from the scenarios marked with `@automated`, you can use `"skipForTags": "not @automated"`. \(Note the space between `not` and the tag name.\)
{% endhint %}

