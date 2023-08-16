# Plugin list

The functionality of SpecSync can be changed or extended with [SpecSync plugins](general-features/specsync-plugins.md). This list contains a few plugins that are ready-to-use to address specific synchronization situations.

If you would like to have your plugin listed here please [contact us](../contact/specsync-support.md).

{% hint style="info" %}
The GitHub repository [https://github.com/specsolutions/specsync-sample-plugins](https://github.com/specsolutions/specsync-sample-plugins) contains several plugins with source-code that can be studied in order to create your own plugin.
{% endhint %}

| Plugin | Description | Maintainer |
|------- | ----------- | ---------- |
| [SpecSync.Plugin.ScenarioOutlinePerExamplesTestCase](https://github.com/specsolutions/specsync-sample-plugins/tree/main/scenario-outline-per-exampes-test-case-plugin) | Plugin that synchronizes scenario outline examples blocks as individual test cases. | Spec Solutions |
| [SpecSync.Plugin.ExcelTestSource](https://github.com/specsolutions/specsync-sample-plugins/tree/main/excel-test-source-plugin) | This plugin can be used to synchronize a local test cases from Excel file using the format that Azure DevOps uses when you export Test Cases to CSV. | Spec Solutions |
| [SpecSync.Plugin.ExcelTestResults](https://github.com/specsolutions/specsync-sample-plugins/tree/main/excel-test-results-plugin) | This plugin can be used to synchronize a local test cases from Excel file using the format that Azure DevOps uses when you export Test Cases to CSV. | Spec Solutions |
| [SpecSync.Plugin.MsTestTestSource](https://github.com/specsolutions/specsync-sample-plugins/tree/main/mstest-test-source-plugin) | Allows synchronizing "C# MsTest Tests" and publish results from TRX result files. | Spec Solutions |
| [SpecSync.Plugin.NUnitTestSource](https://github.com/specsolutions/specsync-sample-plugins/tree/main/nunit-test-source-plugin) | Allows synchronizing "C# NUnit Tests" and publish results from TRX result files. | Spec Solutions |
| [SpecSync.Plugin.ScenarioOutlineAsNormalTestCase](https://github.com/specsolutions/specsync-sample-plugins/tree/main/scenario-outline-as-normal-test-case-format-plugin) | A SpecSync plugin that synchronizes scenario outlines as normal (non-data-driven) Test Cases. | Spec Solutions |
| [SpecSync.Plugin.GenericTestResultMatcher](https://github.com/specsolutions/specsync-sample-plugins/tree/main/generic-test-result-matcher-plugin) | A SpecSync plugin that can be used to override test result matching rules. | Spec Solutions |
| [SpecSync.Plugin.PostmanTestSource](https://github.com/specsolutions/specsync-sample-plugins/tree/main/postman-test-source-plugin) | This plugin can be used to synchronize a tests from a [Postman](https://www.postman.com/) collection and publish results executed with Newman. See [related guide](../important-concepts/using-specsync-with-postman.md) | Spec Solutions |
| [SpecSync.Plugin.TestNGTestSource](https://github.com/specsolutions/specsync-sample-plugins/tree/main/testng-test-source-plugin) | Allows synchronizing Java TestNG test methods and publish test result files. See [related guide](../important-concepts/using-specsync-with-testng.md) | Spec Solutions |
