# SpecSync plugins

SpecSync plugins can extend the functionality of SpecSync in order to support additional test sources, BDD tools and platforms.

SpecSync plugins can be implemented as NuGet packages containing .NET Standard 2.0 assemblies that are loaded by SpecSync during execution. In order to load a plugin, the plugin package has to be registered in the `toolSettings/plugins` section of the configuration file. The list of all possible settings can be found in the [`toolSettings` configuration reference](../../reference/configuration/configuration-toolsettings.md).

{% hint style="info" %}
The GitHub repository [https://github.com/specsolutions/specsync-sample-plugins](https://github.com/specsolutions/specsync-sample-plugins) contains several plugins with source-code that can be used directly for special synchronization situations or studied in order to create your own plugin.
{% endhint %}

The following example shows a simple plugin registration.

{% code title="specsync.json" %}
```javascript
{
  ...
  "toolSettings": {
    "plugins": [
      {
        "packageId": "MySpecSyncPlugin",
        "packageVersion": "1.0.0",
        "parameters": {
          "pluginParameter1":  "value1" 
        } 
      }
    ]
  }, 
  ...
}
```
{% endcode %}

When registering a plugin, additional plugin parameters can be specified. Please contact the maintainer of the plugin for the specific values.

The plugins are loaded from nuget.org by default, but you can specify a folder or an alternative NuGet package feed in the `packageSource` setting. If a plugin is loaded from a package feed SpecSync verifies the package and if it is not signed by Spec Solutions, you will be asked to confirm that you trust the package. To avoid the confirmation prompt, you can set the `toolSettings/allowDownloadingThirdPartyPlugins` setting to `true`. The downloaded plugins are cached to the SpecSync app data folder. The plugin cache folder can be changed using the `toolSettings/pluginCacheFolder` setting.

{% hint style="info" %}
Before SpecSync v3.4, the plugins were only loaded from local assemblies using the `assemblyPath` setting of the plugin configuration. This setting is still available, but loading the plugins from NuGet packages is more convenient in most of the cases.
{% endhint %}

The following example shows how to load a SpecSync plugin from a private package feed.

{% code title="specsync.json" %}
```javascript
{
  ...
  "toolSettings": {
    "plugins": [
      {
        "packageId": "MySpecSyncPlugin",
        "packageVersion": "1.0.0",
        "packageSource": "https://www.myget.org/F/my-feed/api/v3/index.json"
      }
    ]
  }, 
  ...
}
```
{% endcode %}

## Creating SpecSync plugins

The SpecSync plugins have to have a reference to the NuGet package [`SpecSync.PluginDependency`](https://www.nuget.org/packages/SpecSync.PluginDependency) of the version that matches to the installed SpecSync version. This package contains the required interfaces and classes to create a SpecSync plugin. (Before SpecSync v3.4, the package [`SpecSync.AzureDevOps.PluginDependency`](https://www.nuget.org/packages/SpecSync.AzureDevOps.PluginDependency) had to be used.)

If the plugin has further dependencies (e.g. depends on a custom parser), those have to be packaged to the plugin NuGet package. You can find an examle for this in the [Excel Test Source plugin](https://github.com/specsolutions/specsync-plugins/blob/main/excel-test-source-plugin/SpecSync.Plugin.ExcelTestSource/SpecSync.Plugin.ExcelTestSource.csproj#L41).

In order to test the plugins you might need to load multiple compilations of the same plugin package version. By default SpecSync caches the plugin packages by version, so for development we recommend disabling plugin cache using the `toolSettings/disablePluginCache` setting.

The plugin assembly has to contain an assembly-level `SpecSyncPlugin` attribute, that specifies the plugin class (e.g. `[assembly: SpecSyncPlugin(typeof(CustomTestResultMatchPlugin))]`). The plugin class has to implement the `ISpecSyncPlugin` interface that defines a single `Initialize` method.

The plugin can register different services using the `ServiceRegistry` property of the `PluginInitializeArgs` passed in to the `Initialize` method. One plugin can register multiple services.

The `PluginInitializeArgs.Configuration` property can be used to modify or extend the configuration the user has provided (e.g. automatically register a custom field updater). The configuration can only be changed in the `Initialize` method. Changing it later might have no impact or cause inconsistencies.

The following plugin class registers a custom test result matcher service.

{% code title="CustomTestResultMatchPlugin.cs" %}
```csharp
using System;
using MyCustomTestResultMatch.SpecSyncPlugin;
using SpecSync.Plugins;

[assembly: SpecSyncPlugin(typeof(CustomTestResultMatchPlugin))]

namespace MyCustomTestResultMatch.SpecSyncPlugin;

public class CustomTestResultMatchPlugin : ISpecSyncPlugin
{
    public string Name => "Custom Test Result Support";

    public void Initialize(PluginInitializeArgs args)
    {
        args.Tracer.LogVerbose("Initializing custom plugin...");
        args.ServiceRegistry.TestResultMatcher.Register(new CustomTestResultMatcher(), ServicePriority.High);
    }
}
```
{% endcode %}

## Services that can be registered by a plugin

The following table contains the list of services that can be registered by a SpecSync plugin.

| Service | Description |
| ------- | ----------- |
| Synchronization Project Loader | Collects the list of source document references that is processed by SpecSync. This is a simple service, in the most simple case it just needs to list all source files from a folder. The "Source Reference" does not necessarily have to be a physical file (it can also be a namespace or a class). |
| Source Document Parser | The term "Source Document" is a generalized concept for feature files. In case of Gherkin, this is basically the Gherkin parser. The result of the parsing is a set of "local artifact" ("local test case" or "local requirement"), that exposes only a few common properties (e.g. "Name"), but can wrap the result of the parsing (e.g. the Scenario objects in case of Gherkin). The local test case can optionally implement the `IAutomationSettingsProvider` to provide Azure DevOps Test Case automation details. |
| Local Artifact Analyzer | The analysis service takes the "local artifact" ("local test case" or "local requirement") and produces an "artifact sync data" structure that is basically the representation of the part of the Azure DevOps work item that SpecSync can change. For Gherkin, the analyzer service is the one that for example processes all the tags and selects the ones the represent links, the ones that contain the Test Case ID, etc. While the parsing service is general, the analysis service can take the SpecSync configuration into account. |
| Source Document Updater | This service is responsible to write back the changes (e.g. the test case ID after linking) to the source document. This service has to be registered in the Source Document. |
| Test Result Loader | This service loads the test execution result files so that it can be connected with the Test Cases and published to Azure DevOps. |
| Test Result Matcher | This service is used by SpecSync to match the test results (loaded by the Test Result Loader) to the test cases (scenarios). E.g. in case of SpecFlow, the test result file contains the method names (e.g. AddTwoNumbers) and it needs to find which scenario was this created from ("Add two numbers"). |

{% hint style="info" %}
Using a plugin that customizes the _Synchronization Project Loader_, _Source Document Parser_, _Local Artifact Analyzer_ or _Source Document Updater_ services requires an [Enterprise license](../../licensing.md). The _Test Results Loader_ and _Test Result Matcher_ services can be customized with Standard license or using the free plan.
{% endhint %}

## Typical Plugin scenarios

The following table describes a few scenarios that can be supported by a SpecSync plugin.

| Goal | Description | License required |
| ---- | ----------- | ---------------- |
| Support a custom BDD tool that produces a TRX result file (e.g. a SpecFlow test runner) | Since SpecSync has built-in support for loading TRX files, in this case only a _Test Result Matcher_ service has to be implemented. | Any |
| Support a custom BDD tool with an arbitrary test result file format | In this case the plugin needs to implement a _Test Results Loader_ and a _Test Result Matcher_ services. For JUnit XML style result files, the build-in `JUnitXmlResultLoaderBase` base class can also be used to create the custom loader. | Any |
| Support a custom Gherkin dialect | The plugin needs to implement a _Source Document Parser_. | Enterprise |
| Support a custom (non-Gherkin based) test source, e.g. an RSpec test | Probably all services need to be customized. If the test execution uses a standard format (e.g. TRX), the _Test Results Loader can be skipped. | Enterprise |

{% hint style="info" %}
In order to get advise for your goal, [contact SpecSync support](../../contact/specsync-support.md). We are happy to help.
{% endhint %}
