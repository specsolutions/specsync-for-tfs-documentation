# `synchronization/pull` Configuration

This configuration section contains settings to configure the pull behavior.

Read more about the pull behavior in the [Two-way synchronization](../two-way-synchronization.md) concept description. 

*Note: The two-way synchronization is an [Enterprise feature](../licensing.md).* 

The following example shows the available options within this sub-section.

```
{
  ...
  "synchronization": {
	...
    "pull": {
      "enabled": true,
      "enableCreatingScenariosForNewTestCases": false
    },
	...
  },
  ...
}
```


## Settings

* `enabled` -- Enables changing the scenarios in the local repository based on the remote test cases. (Default: `false`)
* `enableCreatingScenariosForNewTestCases` -- Enables creating new scenarios from test cases that are not linked to any scenarios yet. (Default: `false`)


*[Back to the [`synchronization` Configuration](configuration-synchronization.md)]*

*[Back to the [Configuration guide](../configuration.md)]*