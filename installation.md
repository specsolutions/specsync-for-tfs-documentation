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
* specsync4azuredevops.cmd -- this file makes it easier to execute the synchronization tool. Alternatively you can also invoke the `SpecSync4AzureDevOps.exe` directly from the NuGet packages folder. \(See more details in the [Usage](reference/command-line-reference.md) page.\)

## For any platforms \(e.g. for Cucumber\)

SpecSync can be downloaded as a ZIP file, the file contains the synchronization tool inside the `tools` folder. The download links can be found on the [Downloads](downloads.md) page.

Prerequisites for running the synchronization tool.

* On Windows systems: .NET framework 4.5 or later
* On OSX/Linux: Mono

If you need to use SpecSync but cannot ensure the prerequisites, please contact support \(specsync@specsolutions.eu\).

## Install SpecSync as .NET Core tool

{% hint style="info" %}
This install method is available for machines with .NET Core SDK 2.1 installed regardless of the .NET version of the project that is going to be synchronized. If this cannot be ensured, you can use the the [SpecSync.AzureDevOps.Console](https://www.nuget.org/packages/SpecSync.AzureDevOps.Console) NuGet package or [download ](downloads.md)one of the pre-compiled binaries.
{% endhint %}

{% hint style="info" %}
Installing SpecSync as a .NET Core tool is available from SpecSync v2.2 or later \(including v2.2 pre-releases\).
{% endhint %}

The most convenient way to use SpecSync is to install it as a local .NET Core tool. This way you can use different SpecSync versions for different projects and the required SpecSync version is registered in the project repository. You can read more about .NET Core local tools on [Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/core/tools/global-tools#install-a-local-tool). 

.NET Core local tools are only supported with .NET Core SDK 3.0 or later. In case you only have .NET Core 2.1 SDK installed, you can install SpecSync as a [global tool](https://docs.microsoft.com/en-us/dotnet/core/tools/global-tools#install-a-global-tool) or use the [SpecSync.AzureDevOps.Console](https://www.nuget.org/packages/SpecSync.AzureDevOps.Console) NuGet package.

### Step 1 - Initialize .NET Core local tool configuration \(if needed\)

If you haven't used any .NET Core local tool in your project, you need to create the necessary configuration file. Otherwise this step can be skipped.

For initializing the configuration files, you need to run the `dotnet new tool-manifest` command from the solution or repository root directory.

```bash
dotnet new tool-manifest
```

 This command creates a manifest file named `dotnet-tools.json` under the `.config` directory.

### Step 2 - Install SpecSync as a .NET Core local tool

Once the .NET local tool configuration is initialized SpecSync can be easily installed using the `dotnet tool install` command.

```bash
dotnet tool install SpecSync.AzureDevOps --version 2.2.0-pre20200414
```

{% hint style="info" %}
The `--version` setting is only required until the .NET Core tool support is only available as pre-release. It is recommended to specify the latest pre-release version listed on [NuGet.org](https://www.nuget.org/packages/SpecSync.AzureDevOps).
{% endhint %}

### Step 3 - Verify installation

SpecSync is ready to run using the `dotnet specsync` command. You can test the installation by checking the installed SpecSync version.

```bash
dotnet specsync version
```

### Step 4 - Restore SpecSync for other developers working with your project

To be able to use the .NET Core local tools you have installed by other developers of the same project, they need to "restore" the installed tools. This can be performed using the `dotnet tool restore` command, that restores all .NET Core local tools of the project. See more information about this command on [Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/core/tools/global-tools#install-a-local-tool).

```bash
dotnet tool restore
```

