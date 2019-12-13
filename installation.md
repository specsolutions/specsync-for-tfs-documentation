# Installation

For a detailed installation instructions, please check the [Getting started](getting-started/) guide.

## For .NET \(SpecFlow\) projects

For .NET \(SpecFlow\) projects, SpecSync can be installed via NuGet. The synchronization tool can be installed by adding the [`SpecSync.AzureDevOps`](https://www.nuget.org/packages/SpecSync.AzureDevOps) package from NuGet.org.

```text
PM> Install-Package SpecSync.AzureDevOps
```

For synchronizing automated test cases using the "Test Suite based execution" strategy with MsTest and NUnit, an SpecFlow plugin has to be installed additionally. The name of the package depends on the SpecFlow version you use, e.g. for SpecFlow v2.3 the [`SpecSync.AzureDevOps.SpecFlow.2-3`](https://www.nuget.org/packages/SpecSync.AzureDevOps.SpecFlow.2-3) package has to be used. See more details in [Synchronizing automated test cases](important-concepts/synchronizing-automated-test-cases.md).

### Notes for installing SpecSync to SDK-Style .NET Projects \(e.g. .NET Core\)

The [`SpecSync.AzureDevOps`](https://www.nuget.org/packages/SpecSync.AzureDevOps) NuGet package contains the synchronization tool, but it also contains some supporting files to get started with SpecSync easier. Unfortunately when using NuGet for SDK-Style .NET Projects \(e.g. .NET Core\), the NuGet infrastructure does not allow including editable content files in the target project, so these helper files are not added to the project by default. You can add these files manually from the project from the `content` folder of the NuGet package.

* `specsync.json` -- this is a default configuration file. Alternatively you can also create a new JSON file based on the the samples shown in the [Configuration ](configuration/)page of the documentation.
* specsync4azuredevops.cmd -- this file makes it easier to execute the synchronization tool. Alternatively you can also invoke the `SpecSync4AzureDevOps.exe` directly from the NuGet packages folder. \(See more details in the [Usage](usage.md) page.\)

## For any platforms \(e.g. for Cucumber\)

SpecSync can be downloaded as a ZIP file, the file contains the synchronization tool inside the `tools` folder. The download links can be found on the [Downloads](downloads.md) page.

Prerequisites for running the synchronization tool.

* On Windows systems: .NET framework 4.5 or later
* On OSX/Linux: Mono

If you need to use SpecSync but cannot ensure the prerequisites, please contact support \(specsync@specsolutions.eu\).

