# `synchronization/format` Configuration

This configuration section contains settings to configure the format of the synchronized test case.

Read more about the test case format options in the [Configuring the format of the synchronized test cases](../configuring-the-format-of-the-synchronized-test-cases.md) concept description. 

The following example shows the available options within this sub-section.

```
{
  ...
  "synchronization": {
	...
    "format": {
      "useExpectedResult": false,
      "syncDataTableAsText": false,
      "prefixBackgroundSteps": true,
      "prefixTitle": true
    }
  },
  ...
}
```


## Settings

* `useExpectedResult` -- If set to true, *Then* steps will be synchronized to the *Expected result* field of the test case steps. (Default: `false`)
* `syncDataTableAsText` -- If set to true, DataTables will be synchronized as plain text instead of HTML tables. (Default: `false`)
* `prefixBackgroundSteps` -- If set to true, *Background* steps will be synchronized with the `Background:` prefix. (Default: `true`)
* `prefixTitle` -- If set to true, test case title will be synchronized with the `Scenario:` or `Scenario Outline:` prefix. (Default: `true`)


*[Back to the [`synchronization` Configuration](configuration-synchronization.md)]*

*[Back to the [Configuration guide](../configuration.md)]*