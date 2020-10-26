# How to synchronize automated test cases

Automation is an important element of BDD as it helps verifying whether the implementation fulfills the required expectations or not. When the BDD scenarios are synchronized to Azure DevOps, the created Test Cases should indicate that they are representing automated tests. In order to achieve complete traceability and a form of "living documentation" the results of the scenario executions should also be published to the synchronized Test Cases.

SpecSync supports synchronizing automated test cases to achieve both of these goals.

### Mark Test Cases as Automated

SpecSync can be configured to mark the synchronized Test Cases as _Automated_. You can enable this feature for all Test Cases, but you can also exclude scenarios if your feature files contain both automated and manual scenarios. 

The documentation page [Mark Test Cases as Automated](../features/push-features/mark-test-cases-as-automated.md) contains the details about how this can be configured.

{% hint style="info" %}
After changing the automation configuration, you need to perform the push command with an additional `--force` setting. Using the `--force` setting is only required once.
{% endhint %}

### Publish test results to the synchronized Test Cases

The SpecSync publish-test-result command can be used to publish test result files of scenario executions to Azure DevOps. SpecSync will analyze the test result file, match the results to the scenarios and publishes the results to the Test Cases connected to the matched scenarios.

In the most simple case, you only need a test result file and a selected Azure DevOps Test Configuration to publish the results, but there are plenty of ways how the command can be customized. 

The [Publishing test result files](../features/test-result-publishing-features/publishing-test-result-files.md) page describes the test result publishing concept in detail and shows the customization options.

With the publish-test-result command, you can publish local test results as well, but in the majority of the cases this command is invoked from a build or release pipeline. The [Use SpecSync from build or release pipeline](synchronizing-test-cases-from-build.md) page shows how to configure the pipeline for this.

