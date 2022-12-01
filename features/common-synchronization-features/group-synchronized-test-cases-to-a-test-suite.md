# Include synchronized Test Cases to a Test Suite

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
_The test suite names are not unique in Azure DevOps. If the same name is used for multiple suites, you should specify the test suite ID in the_ `remote/testSuite/id` _setting instead._
{% endhint %}

{% hint style="info" %}
Retrieving Test Suites might take longer time when the ID of the Test Plan that the Test Suite is defined in is unknown. If you have many Test Plans or experience longer processing time, please specify the ID of the Test Plan as well in the `remote/testSuite/testPlanId` setting.
{% endhint %}

