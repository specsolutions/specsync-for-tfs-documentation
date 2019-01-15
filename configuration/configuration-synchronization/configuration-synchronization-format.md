# format

This configuration section contains settings to configure the format of the synchronized test case.

Read more about the test case format options in the [Configuring the format of the synchronized test cases](../../important-concepts/configuring-the-format-of-the-synchronized-test-cases.md) concept description.

The following example shows the available options within this sub-section.

```javascript
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

* `useExpectedResult` -- If set to true, _Then_ steps will be synchronized to the _Expected result_ field of the test case steps. \(Default: `false`\)
* `syncDataTableAsText` -- If set to true, DataTables will be synchronized as plain text instead of HTML tables. \(Default: `false`\)
* `prefixBackgroundSteps` -- If set to true, _Background_ steps will be synchronized with the `Background:` prefix. \(Default: `true`\)
* `prefixTitle` -- If set to true, test case title will be synchronized with the `Scenario:` or `Scenario Outline:` prefix. \(Default: `true`\)

_\[Back to the_ [`synchronization` _Configuration_](./)_\]_

_\[Back to the_ [_Configuration guide_](../)_\]_

