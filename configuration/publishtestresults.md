# publishTestResults

This configuration section contains settings related to publishing TRX test results. 

To read more about publishing test results see the "Assembly based execution strategy" section of the [Synchronizing automated test cases](../important-concepts/synchronizing-automated-test-cases.md) article.

The following example shows the available options within this section.

```javascript
{
  ...
  "publishTestResults": {
    "testConfiguration": {
      "name": "Windows 10"
    },
    "testResult": {
      "filePath": "test-result.trx"
    }
  },
  ...
}
```

## Settings:

* `testConfiguration` -- Specifies a test configuration within the Azure DevOps project as a target configuration for publishing test results.
  * `testConfiguration/name` -- The name of the test configuration.
  * `testConfiguration/id` -- The ID of the test configuration.
* `testResult` -- The TRX test result configuration.
  * `testResult/filePath` -- The path of the TRX file. Can also be specified as a command line parameter.

