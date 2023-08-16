# Using SpecSync with TestNG

[TestNG](https://testng.org/) is a popular Java testing framework.

SpecSync can be used to synchronize TestNG test methods with Azure DevOps Test Cases and publish their test results from Surefire XML or JUnit XML result files.

In order to use SpecSync with TestNG tests, you need to enable the [`SpecSync.Plugin.TestNGTestSource`](https://github.com/specsolutions/specsync-sample-plugins/tree/main/testng-test-source-plugin) plugin by adding the following section to the SpecSync configuration file (`specsync.json`).

```javascript
  "toolSettings": {
    "plugins": [
      {
        "packageId": "SpecSync.Plugin.TestNGTestSource",
        "packageVersion": "1.0.0", // check the latest version at https://www.nuget.org/packages/SpecSync.Plugin.TestNGTestSource
      }
    ]
  },
```

You might need to set other settings a well, as described below. You can find a complete configuration sample in the [*Sample SpecSync configuration file for synchronizing TestNG tests* section](#sample-config) below.

{% hint style="info" %}
The TestNG Test Source_ plugin described here is an [Enterprise feature](../licensing.md).
{% endhint %}

## Synchronizing Postman Tests

The plugin processes the Java files in the configured folder tree and searches for *TestNG* test methods, like:

```
@Test(groups = { "MyGroup" })
public void onePassingTest() {
    ...
}
```

These test methods are the potential local test cases to be synchronized. 

Once the methods are linked to a newly created Azure DevOps Test Case, the Test Case ID is inserted into the 
Java file as a group in the `@Test` annotation using a "SpecSync tag" (see below) like:

```
@Test(groups = { "MyGroup", "tc:234" })
public void onePassingTest() {
    ...
}
```

#### Specifying tags for SpecSync

The TestNG groups specified using the `@Test` annotation will be used as SpecSync tags. 

You can also define common tags for all methods of a class by adding a `@Test` annotation to the class (as well).

The tag that provides the link to the synchronized Test Case is by default of format `tc:[test-case-id]`, but both 
the `tc` prefix and the `:` separator can be configured to be different in `specsync.json` "synchronization" section.

The tags that establish a link to another work item are in format `[prefix]:[work-item-id]`, where the prefix is 
configured in `specsync.json` "synchronization/links" section.

The following example shows a test method that is linked to a User Story.


```
@Test(groups = { "my_tag", "tc:234", "story:123" })
public void onePassingTest() {
    ...
}
```

#### Synchronizing method description and source code

The sycnrhonization can be configured in a way that it updates a Test Case field (e.g. "Description") with the method 
description and/or the source code of the method. The method description is the text specified as Doc Comment for the method.

The following `specsync.json` configuration setting enables updating the "Description" field with the method description and the source code.

```
{
  ...
  "synchronization": {
    ...
    "fieldUpdates": {
      "Description": "{local-test-case-description}{br}<pre>{local-test-case-container-source-file-path:HtmlEncode}:{br}{local-test-case-source:HtmlEncode}</pre>"
    }
  }
}
```


## Publishing Test Results

The plugin can also publish test execution results to the synchronized Test Cases. For that first you need to run the TestNG tests and produce a test result file. 

Currently the plugin can process the following test result files:

#### Maven Surefire XML result file

The Maven Surefire XML result file can be found typically at `target\surefire-reports\TEST-TestSuite.xml`). To be able to 
publish the result you need to use the `SurefireXml` format and specify the path of the XML file, e.g.

```
dotnet specsync publish-test-results -r .\target\surefire-reports\TEST-TestSuite.xml -f SurefireXml
```

#### JUnit XML result file

The TestNG test results are also saved to "JUnit" compatible test result files as well, typically saved to `target\surefire-reports\junitreports\`. 

To be able to publish the results from these files, you need to the the `JUnitXml` format and specify the folder name, e.g.

```
dotnet specsync publish-test-results -r .\target\surefire-reports\junitreports\ -f JUnitXml
```

*Note:* The Surefire XML file contains more information about the data-driven tests, so if both are available, we recommend 
to use the Surefire XML result file.

