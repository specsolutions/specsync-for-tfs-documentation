# Include synchronized Test Cases to a Test Suite (deprecated)

{% hint style="warning" %}
With SpecSync v5 the `remote/testSuite` setting described here has been deprecated. 
For including synchronized Test Cases to a Test Suite, please use the [Synchronizing Test Case hierarchies using Test Suites](synchronizing-test-case-hierarchies.md) feature. 
For ensuring that SpecSync can detect local test case deletions, please use the [Remote scope](remote-scope.md) feature.
The [Upgrade configuration wizard](../general-features/configuration-wizards.md#upgrade-wizard) can also be used to upgrade this setting for v5.
{% endhint %}

To be able to easily find these synchronized test cases in Azure DevOps, they can be added to test suites. SpecSync can automatically add/remove the synchronized test cases to a test suite. For this you have to specify the name or the ID or the name of the test suite in the configuration.

1. Create a "Static suite" \(e.g. "BDD Scenarios"\) in Azure DevOps. \(For that you have to navigate to "Test plans" and create and select a test plan first.\)
2. Specify the name of the test suite in the `remote/testSuite/name` entry of the `specsync.json` file. \(Alternatively you can specify the ID of the suite in `remote/testSuite/id`. The suite names are not unique in Azure DevOps!\)

   ```text
   {
     "$schema": "http://schemas.specsolutions.eu/specsync4azuredevops-config-latest.json",

     // See configuration options and samples at http://speclink.me/specsyncconfig.

     "remote": {
       "projectUrl": "https://specsyncdemo.visualstudio.com/MyCalculator",
       "user": "52yny4a......................................ycsetda",
       "testSuite": {
         "name": "BDD Scenarios"
       }
     }
   }
   ```

3. Make sure that the project compiles and the tests pass.
4. Run the synchronization again:

   ```text
   path-to-specsync-package\tools\SpecSync4AzureDevOps.exe push
   ```

As a result, SpecSync adds the synchronized test cases to the test suite.

![Test cases were added to the test suite](../../.gitbook/assets/getting-started-specflow-updated-test-suite.png)

If you specify a test suite name that does not exist, SpecSync creates a test suite first \(under the first test plan\).

{% hint style="info" %}
The test suite names are not unique in Azure DevOps. If the same name is used for multiple suites, you should specify the test suite ID in the `remote/testSuite/id` setting instead.
{% endhint %}

{% hint style="info" %}
Retrieving Test Suites might take longer time when the ID of the Test Plan that the Test Suite is defined in is unknown. If you have many Test Plans or experience longer processing time, please specify the ID of the Test Plan as well in the `remote/testSuite/testPlanId` setting.
{% endhint %}

## Removing Test Cases from the Test Suite

When a scenario is removed from the feature file, SpecSync will also remove the linked Test Case from the Test Suite. In that case the Test Case related to the missing scenario is not deleted but tagged with `specsync:removed`. You can review the Test Cases of this tag and delete them manually. (The tag is removed from the Test Case if the scenario is restored.)

{% hint style="info" %}
Scenario removals are detected, but the Test Cases will not be removed from the Test Suite when the synchronization is performed with a filter (e.g. by using the `--tagFilter` command line option). The actual removal will be performed at the next full (unfiltered) synchronization.
{% endhint %}
