# `synchronization/automation` Configuration

This configuration section contains settings to configure synchronizing automated test cases. 

Read more about synchronizing automated test cases in the [Synchronizing automated test cases](../synchronizing-automated-test-cases.md) concept description. 

The following example shows the available options within this sub-section.

```
{
  ...
  "synchronization": {
	...
    "automation": {
      "enabled": true,
      "skipForTags": "@manual"
    },
	...
  },
  ...
}
```


## Settings

* `enabled` -- Specifies whether SpecSync should attempt to create automated test cases. (Default: `false`)
* `skipForTags` -- A [tag expression](http://speclink.me/tagexpressions) of scenarios that should be excluded from automation (e.g. `@manual or @planned`). (Default: *[all test cases synced as automated]*)


*[Back to the [`synchronization` Configuration](configuration-synchronization.md)]*

*[Back to the [Configuration guide](../configuration.md)]*