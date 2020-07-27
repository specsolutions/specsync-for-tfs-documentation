# toolSettings

This configuration section contains settings for the synchronization tool.

The following example shows the available options within this section.

```javascript
{
  ...
  "toolSettings": {
    "licensePath": "specsync.lic",
    "disableStats": false,
    "outputLevel": "normal"
  }, 
  ...
}
```

## Settings

* `licensePath` -- Path for the license file. Can contain an absolute or a relative path to the config file folder. It may contain environment variables in `...%MYENV%...` form. Can be overridden by the `--license` [command line option](../reference/command-line-reference.md). See [Licensing](../licensing.md) for details. \(Default: `specsync.lic`\) 
* `disableStats` -- If set to true, SpecSync will not collect anonymous error diagnostics and statistics. Can be overridden by the `--disableStats` [command line option](../reference/command-line-reference.md). \(Default: `false`\)
* `outputLevel` -- Set the detail level of error messages and trace information displayed by the tool. Available options: `normal`, `verbose` and `debug`. Can be overridden by the `--verbose` [command line option](../reference/command-line-reference.md). \(Default: `normal`\)

_\[Back to the_ [_Configuration guide_](./)_\]_

