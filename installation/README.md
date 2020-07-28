# Installation & Usage

{% hint style="warning" %}
Installation options have been changed in SpecSync v3. For older releases check the [Older versions](../reference/older-versions.md) reference page.
{% endhint %}

SpecSync for Azure DevOps can be installed in different ways depending on the project context. The following table contains an overview of the installation options. The detailed installation instructions can be found by clicking the name of the option.

| Option | Supported Platforms | Prerequisites | Notes |
| :--- | :--- | :--- | :--- |
| [.NET Core tool](dotnet-core-tool.md) \(NuGet, Download\) | Windows, Linux, MacOS, Docker | .NET Core 3.0 SDK or later | Recommended |
| [.NET Console App](dotnet-console.md) \(NuGet, Download\) | Windows | .NET Framework 4.6.2 or later | Compatible with SpecSync v2.1 usage |
| [Native binaries for Linux or MaxOS](native-binaries.md) \(Download\) | Linux, MacOS | - | Larger package \(~35MB\) |

Once SpecSync has been installed, you need to setup and configure it before the first use. Check the [Setup and Configure](setup-and-configure.md) page for the details. 

{% page-ref page="setup-and-configure.md" %}

{% hint style="info" %}
For setting up SpecSync for a new project, you can also check the [Getting started](../getting-started/) guide.
{% endhint %}





## For .NET \(SpecFlow\) projects

For .NET \(SpecFlow\) projects, SpecSync can be installed via NuGet. The synchronization tool can be installed by adding the [`SpecSync.AzureDevOps`](https://www.nuget.org/packages/SpecSync.AzureDevOps) package from NuGet.org.

```text
PM> Install-Package SpecSync.AzureDevOps
```

For synchronizing automated test cases using the "Test Suite based execution" strategy with MsTest and NUnit, an SpecFlow plugin has to be installed additionally. The name of the package depends on the SpecFlow version you use, e.g. for SpecFlow v2.3 the [`SpecSync.AzureDevOps.SpecFlow.2-3`](https://www.nuget.org/packages/SpecSync.AzureDevOps.SpecFlow.2-3) package has to be used. See more details in [Synchronizing automated test cases](../important-concepts/synchronizing-automated-test-cases.md).

### Notes for installing SpecSync to SDK-Style .NET Projects \(e.g. .NET Core\)

The [`SpecSync.AzureDevOps`](https://www.nuget.org/packages/SpecSync.AzureDevOps) NuGet package contains the synchronization tool, but it also contains some supporting files to get started with SpecSync easier. Unfortunately when using NuGet for SDK-Style .NET Projects \(e.g. .NET Core\), the NuGet infrastructure does not allow including editable content files in the target project, so these helper files are not added to the project by default. You can add these files manually from the project from the `content` folder of the NuGet package.

* `specsync.json` -- this is a default configuration file. Alternatively you can also create a new JSON file based on the the samples shown in the [Configuration ](../reference/configuration/)page of the documentation.
* specsync4azuredevops.cmd -- this file makes it easier to execute the synchronization tool. Alternatively you can also invoke the `SpecSync4AzureDevOps.exe` directly from the NuGet packages folder. \(See more details in the [Usage](../reference/command-line-reference/) page.\)

## For any platforms \(e.g. for Cucumber\)

SpecSync can be downloaded as a ZIP file, the file contains the synchronization tool inside the `tools` folder. The download links can be found on the [Downloads](../downloads.md) page.

Prerequisites for running the synchronization tool.

* On Windows systems: .NET framework 4.5 or later
* On OSX/Linux: Mono

If you need to use SpecSync but cannot ensure the prerequisites, please contact support \(specsync@specsolutions.eu\).

## 

