# toolSettings

This configuration section contains settings for the synchronization tool.

The following example shows the available options within this section.

```javascript
{
  ...
  "toolSettings": {
    "licensePath": "specsync.lic",
    "disableStats": false,
    "outputLevel": "normal",
    "parentConfig": "specsync-common.json",
    "ignoreParentConfig": false
  }, 
  ...
}
```

## Settings

| Setting | Description | Default |
| :--- | :--- | :--- |
| `licensePath` | Path for the license file. Can contain an absolute or a relative path to the config file folder. It may contain environment variables in `...%MYENV%...` form. Can be overridden by the `--license` [command line option](../command-line-reference/). See [Licensing](../../licensing.md) for details. | `specsync.lic` |
| `disableStats` | If set to true, SpecSync will not collect anonymous error diagnostics and statistics. Can be overridden by the `--disableStats` [command line option](../command-line-reference/). | `false` |
| `outputLevel` | Set the detail level of error messages and trace information displayed by the tool. Available options: `normal`, `verbose` and `debug`. Can be overridden by the `--verbose` [command line option](../command-line-reference/). | `normal` |
| `parentConfig` | Path for the parent config file. See [Hierarchical configuration files](../../features/general-features/hierarchical-configuration-files.md) for details. | not specified, the files in the parent folder will be considered |
| `ignoreParentConfig` | If set to true, SpecSync will not apply the configuration settings of the config files in the parent folders or the user-specific configuration settings. See [Hierarchical configuration files](../../features/general-features/hierarchical-configuration-files.md) for details. | `false` |

{% page-ref page="./" %}

{% page-ref page="../../features/general-features/hierarchical-configuration-files.md" %}

