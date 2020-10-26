# SpecSync Plugins

{% hint style="warning" %}
The plugins feature of SpecSync is currently in a preview phase. This means that changes in the API might happen in the upcoming SpecSync versions.
{% endhint %}

SpecSync plugins can extend the functionality of SpecSync in order to support additional test sources, BDD tools and platforms.

SpecSync plugins can be implemented as .NET assemblies that are loaded by SpecSync during execution.  In order to load a plugin, the plugin assembly has to be registered in the `toolSettings/plugins` section of the configuration file. The list of all possible settings can be found in the [`toolSettings` configuration reference](../../reference/configuration/configuration-toolsettings.md). 

The following example shows a simple plugin registration.

{% code title="specsync.json" %}
```javascript
{
  ...
  "toolSettings": {
    "plugins": [
      {
        "assemblyPath": "..\\MySpecSyncPlugin\\MySpecSyncPlugin.dll",
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

{% hint style="info" %}
The GitHub repository [https://github.com/specsolutions/specsync-sample-plugins](https://github.com/specsolutions/specsync-sample-plugins) contains sample SpecSync plugins.
{% endhint %}

The SpecSync plugins have to have a reference to the NuGet package [`SpecSync.AzureDevOps.PluginDependency`](https://www.nuget.org/packages/SpecSync.AzureDevOps.PluginDependency) of the version that matches to the installed SpecSync version. This package contains the required interfaces and classes to create a SpecSync plugin.

The plugin assembly has to contain an assembly-level `SpecSyncPlugin` attribute, that specifies the plugin class \(e.g. `[assembly: SpecSyncPlugin(typeof(CustomTestResultMatchPlugin))]`\). The plugin class has to implement the `ISpecSyncPlugin` interface that defines a single `Initialize` method. 

The plugin can register different services using the `ServiceRegistry` property of the `PluginInitializeArgs` passed in to the `Initialize` method. One plugin can register multiple services.

The following plugin class registers a custom test result matcher service.

{% code title="CustomTestResultMatchPlugin.cs" %}
```csharp
using System;
using MyCustomTestResultMatch.SpecSyncPlugin;
using SpecSync.AzureDevOps.Plugins;

[assembly: SpecSyncPlugin(typeof(CustomTestResultMatchPlugin))]

namespace MyCustomTestResultMatch.SpecSyncPlugin
{
    public class CustomTestResultMatchPlugin : ISpecSyncPlugin
    {
        public string Name => "Custom Test Result Support";

        public void Initialize(PluginInitializeArgs args)
        {
            args.Tracer.LogVerbose("Initializing custom plugin...");
            args.ServiceRegistry.TestResultMatcher.Register(new CustomTestResultMatcher(), ServicePriority.High);
        }
    }
}
```
{% endcode %}

## Services that can be registered by a plugin

The following table contains the list of services that can be registered by a SpecSync plugin.

| Service | Description |
| :--- | :--- |
| BDD Project Loader | Collects the list of source files that is processed by SpecSync. This is a simple service, in the most simple case it just needs to list all source files from a folder. The “Source File” does not necessarily have to be a physical file \(it can also be a namespace or a class\). |
| Local Test Case Container Parser | The term “local test case container” is a generalized concept for feature files. In case of Gherkin, this is basically the Gherkin parser. The result of the parsing is a set of “local test case” object, that exposes only a few common properties \(e.g. “Name”\), but can wrap the result of the parsing \(e.g. the Scenario objects in case of Gherkin\).  |
| Local Test Case Analyzer | The analysis service takes the “local test case” \(generalization of “scenario”\) and produces a “test case source data” structure that is basically the representation of the part of the Azure DevOps Test Case that SpecSync can change. For Gherkin, the analyzer service is the one that for example processes all the tags and selects the ones the represent links, the ones that contain the Test Case ID, etc. While the parsing service is general, the analysis service can take the SpecSync configuration into account.  |
| Local Test Case Container Updater | This service is responsible to write back the changes \(e.g. the test case ID after linking\) to the source file. This service has to be registered in the Local Test Case Container. |
| Test Result Loader | This service loads the test execution result files so that it can be connected with the Test Cases and published to Azure DevOps.  |
| Test Result Matcher | This service is used by SpecSync to match the test results \(loaded by the Test Result Loader\) to the test cases \(scenarios\). E.g. in case of SpecFlow, the test result file contains the method names \(e.g. AddTwoNumbers\) and it needs to find which scenario was this created from \(“Add two numbers”\).  |

{% hint style="info" %}
Using a plugin that customizes the _BDD Project Loader_, _Local Test Case Container Parser_, _Local Test Case Analyzer_ or _Local Test Case Container Updater_ services requires an [Enterprise license](../../licensing.md). The _Test Results Loader_ and _Test Result Matcher_ services can be customized with Standard license or using the free plan.
{% endhint %}

## Typical Plugin scenarios

The following table describes a few scenarios that can be supported by a SpecSync plugin.

| Goal | Description | License required |
| :--- | :--- | :--- |
| Support a custom BDD tool that produces a TRX result file \(e.g. a SpecFlow test runner\) | Since SpecSync has built-in support for loading TRX files, in this case only a _Test Result Matcher_ service has to be implemented. | Any |
| Support a custom BDD tool with an arbitrary test result file format | In this case the plugin needs to implement a _Test Results Loader_ and a _Test Result Matcher_ services. For JUnit XML style result files, the build-in `JUnitXmlResultLoader` base class can also be used to create the custom loader. | Any |
| Support a custom Gherkin dialect | The plugin needs to implement a _Local Test Case Container Parser_.  | Enterprise |
| Support a custom \(non-Gherkin based\) test source, e.g. an RSpec test | Probably all services need to be customized. If the test execution uses a standard format \(e.g. TRX\), the _Test Results Loader_ can be skipped_._ | Enterprise |

{% hint style="info" %}
In order to get advise for your goal, [contact SpecSync support](../../contact/specsync-support.md). We are happy to help.
{% endhint %}

