# `synchronization/state` Configuration

This configuration section contains settings to configure how the test case *state* field should be updated.

The following example shows the available options within this sub-section.

```
{
  ...
  "synchronization": {
	...
    "state": {
      "setValueOnChangeTo": "Design"
    },
	...
  },
  ...
}
```


## Settings

* `setValueOnChangeTo` -- A state value (e.g. `Design`) to set test case state to when updating or creating a test case during synchronization. Useful for setting back `Ready` test cases to `Design` on change. (Default: *[don't change test case state]*)


*[Back to the [`synchronization` Configuration](configuration-synchronization.md)]*

*[Back to the [Configuration guide](../configuration.md)]*