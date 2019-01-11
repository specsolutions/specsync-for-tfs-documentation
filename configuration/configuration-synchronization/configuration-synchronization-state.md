# state

This configuration section contains settings to configure how the test case _state_ field should be updated.

The following example shows the available options within this sub-section.

```text
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

* `setValueOnChangeTo` -- A state value \(e.g. `Design`\) to set test case state to when updating or creating a test case during synchronization. Useful for setting back `Ready` test cases to `Design` on change. \(Default: _\[don't change test case state\]_\)

_\[Back to the_ [`synchronization` _Configuration_](./)_\]_

_\[Back to the_ [_Configuration guide_](../)_\]_

