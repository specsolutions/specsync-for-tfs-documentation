# synchronization

This configuration section contains synchronization settings.

The following example shows the available options within this section.

```javascript
{
  ...
  "synchronization": {
    "enableLocalChanges": true,
    "forceUpdate": true,
    "testCaseTagPrefix": "tc",

    "pull": {
      "enabled": true,
      "enableCreatingScenariosForNewTestCases": false
    },

    "automation": {
      "enabled": true,
      "skipForTags": "@manual"
    },

    "state": {
      "setValueOnChangeTo": "Design"
    },

    "areaPath": {
      "mode": "setOnLink",
      "value": "\\MyArea"
    },
    "iterationPath": {
      "mode": "setOnLink",
      "value": "\\Iteration1"
    },
    "links": [
      {
        "targetWorkItemType": "ProductBacklogItem",
        "tagPrefix": "pbi",
        "relationship": "Child",
        "mode": "createIfMissing"
      },
      {
        "tagPrefix": "bug"
      }
    ],
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

* `enableLocalChanges` -- Enables changing feature files in the local repository. If set to false \(called _build server mode_\), only those changes will be performed that do not need any change in the local feature files. Linking new scenarios or pulling changes from test cases will be skipped. Can be overridden by the `--buildServerMode` [command line option](../../reference/usage.md). See [Synchronizing test cases from build](../../important-concepts/synchronizing-test-cases-from-build.md) for details. \(Default: `true`\)
* `forceUpdate` -- If set to true, SpecSync update test cases even if there is no local change and the test case was not modified remotely. Can be overridden by the `--force` [command line option](../../reference/usage.md). \(Default: `false`\)
* `testCaseTagPrefix` -- The tag prefix for specifying the test cases. E.g. specify `testcase` for referring to test cases using a tag, like `@testcase:1234`. \(Default: `tc`\)

## Sub-sections

The following configuration sub-sections can be used. Click to the name of the section for detailed documentation.

* [`pull`](configuration-synchronization-pull.md) -- Settings to configure the pull behavior. See [Two-way synchronization](../../important-concepts/two-way-synchronization.md) for details.
* [`automation`](configuration-synchronization-automation.md) -- Settings to configure synchronizing automated test cases. See [Synchronizing automated test cases](../../important-concepts/synchronizing-automated-test-cases.md) for details.
* [`state`](configuration-synchronization-state.md) -- Settings to configure how the test case _state_ field should be updated.
* [`areaPath`](configuration-synchronization-areapath.md) -- Settings to configure how the test case _area path_ field should be updated. See [Add new test cases to an Area or an Iteration](../../important-concepts/add-new-test-cases-to-an-area-or-an-iteration.md) for details.
* [`iterationPath`](configuration-synchronization-iterationpath.md) -- Settings to configure how the test case _iteration path_ field should be updated. See [Add new test cases to an Area or an Iteration](../../important-concepts/add-new-test-cases-to-an-area-or-an-iteration.md) for details.
* [`links`](configuration-synchronization-links.md) -- Settings to configure how test case links should be updated based on the tags of the scenario. See [Linking work items using tags](../../important-concepts/linking-work-items-with-tags.md) for details.
* [`format`](configuration-synchronization-format.md) -- Settings to configure the format of the synchronized test case. See [Configuring the format of the synchronized test cases](../../important-concepts/configuring-the-format-of-the-synchronized-test-cases.md) for details.

_\[Back to the_ [_Configuration guide_](../)_\]_

