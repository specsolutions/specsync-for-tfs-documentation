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
| ------- | ----------- | ------- |
| `licensePath` | Path for the license file. Can contain an absolute or a relative path to the config file folder. It may contain environment variables in `...%MYENV%...` form. Can be overridden by the `--license` [command line option](../command-line-reference/#common-command-line-options). See [Licensing](../../licensing.md) for details. | `specsync.lic` |
| `evaluationMode` | If set to true, SpecSync will not fail when the license scenario limit is reached but completes the synchronization up to the limit. | `false` |
| `disableStats` | If set to true, SpecSync will not collect anonymous error diagnostics and statistics. Can be overridden by the `--disableStats`  [command line option](../command-line-reference/#common-command-line-options). | `false` |
| `outputLevel` | Set the detail level of error messages and trace information displayed by the tool. Available options: `normal`, `verbose` and `debug`. Can be overridden by the `--verbose` [command line option](../command-line-reference/#common-command-line-options). | `normal` |
| `parentConfig` | Path for the parent config file. See [Hierarchical configuration files](../../features/general-features/hierarchical-configuration-files.md) for details. | not specified, the files in the parent folder will be considered |
| `ignoreParentConfig` | If set to true, SpecSync will not apply the configuration settings of the config files in the parent folders or the user-specific configuration settings. See [Hierarchical configuration files](../../features/general-features/hierarchical-configuration-files.md) for details. | `false` |
| `logFile` | Path for the log file to be created. Can be overridden by the `--log` [command line option](../command-line-reference/#common-command-line-options). | no log file |
| `allowDownloadingThirdPartyPlugins` | If set to true, SpecSync is allowed to download plugin packages created by 3rd parties from nuget.org or the specified package source. | `false` |
| `pluginCacheFolder` | Custom plugin cache folder to be used by SpecSync to download plugin packages. | plugins stored in the SpecSync app data folder | 
| `disablePluginCache` | If set to true, SpecSync will always download or extract the plugin package even if the same plugin package version has been used already. This is useful for testing plugins. | `false` |
| `plugins` | An array of [plugin configurations](#plugin-configuration). See [SpecSync Plugins](../../features/general-features/specsync-plugins.md) for details. | no plugins |

### Plugin configuration 

These settings can be used in the `toolSettings/plugins` array. See [SpecSync Plugins](../../features/general-features/specsync-plugins.md) for details.

| Setting | Description | Default |
| ------- | ----------- | ------- |
| `packageId` | The ID of the plugin package (NuGet) | either this or `assemblyPath` is mandatory |
| `packageVersion` | The full version of the plugin package | mandatory, if `packageId` is specified |
| `packageSource` | he package source (NuGet v3 feed or local folder) to be used to download the plugin package. | uses nuget.org feed |
| `assemblyPath` | The path for the SpecSync plugin assembly (dll file) when the plugin is not loaded from NuGet package | either this or `packageId` is mandatory | 
| `parameters` | Optional parameters for the plugin in key-value pairs. Please contact the maintainer of the plugin for the specific values. |

{% page-ref page="./" %}

{% page-ref page="../../features/general-features/hierarchical-configuration-files.md" %}

