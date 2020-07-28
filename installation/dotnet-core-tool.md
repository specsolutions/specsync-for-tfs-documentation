# Install as .NET Core tool

{% hint style="info" %}
This install method is available for machines with .NET Core SDK 3.0 or higher installed regardless of whether the BDD project uses .NET or whether it uses .NET Core. Other [installation options](./) available.
{% endhint %}

{% hint style="warning" %}
The Azure DevOps API that SpecSync uses currently does not support[ "Microsoft account sign-in prompt" authentication](../important-concepts/tfs-authentication-options.md#tfs-windows-sign-in-prompt) on .NET Core. The other authentication options, including the recommended [Personal Access Token \(PAT\)](../important-concepts/tfs-authentication-options.md#vsts-personal-access-tokens) or password authentication is still supported.
{% endhint %}

The most convenient way to use SpecSync is to install it as a .NET Core tool. While .NET Core tools require .NET Core SDK to be installed, that can be installed to any platforms, including Linux and MacOS. It also supports execution in Docker containers. .NET Core SDK can be installed from the [.NET Core Download](https://dotnet.microsoft.com/download) page. We recommend installing the latest .NET Core SDK.

SpecSync can be installed as .NET Core tool, even if the project does not use .NET \(e.g. uses Cucumber Java\) or if it uses a different version of .NET \(e.g. .NET Framework v4.7\).

SpecSync should be installed as a _local_ .NET Core tool \(.NET Core tools can also be installed globally for the machine, but that is more suitable for general, non project specific tools\). By installing as local tool, you can use different SpecSync versions for different projects and the required SpecSync version is registered in the project repository. You can read more about .NET Core local tools on [Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/core/tools/global-tools#install-a-local-tool).

### Step 1: Initialize .NET Core local tool configuration \(if needed\)

If you haven't used any .NET Core local tool in your project, you need to create the necessary configuration file. Otherwise this step can be skipped.

For initializing the configuration files, you need to run the `dotnet new tool-manifest` command from the solution or repository root directory.

```bash
dotnet new tool-manifest
```

This command creates a manifest file named `dotnet-tools.json` under the `.config` directory. This file should be added to source control so that all other members of the team can use the same tools.

### Step 2: Install SpecSync as a .NET Core local tool

Once the .NET local tool configuration is initialized SpecSync can be easily installed using the `dotnet tool install` command. This will download and install the latest version of SpecSync from NuGet.org.

```bash
dotnet tool install SpecSync.AzureDevOps
```

### Step 3: Verify installation

SpecSync is ready to run using the `dotnet specsync` command. You can test the installation by checking the installed SpecSync version.

```bash
dotnet specsync version
```

If the correct version number is displayed, you are ready to move on to setup and configure SpecSync for the first synchronization. Check the [Setup and Configure](setup-and-configure.md) page for the details.

### Step 4: Restore SpecSync for other people working with your project

To be able to use the .NET Core local tools you have installed by other members of the same project, they need to "restore" the installed tools. This can be performed using the `dotnet tool restore` command, that restores all .NET Core local tools of the project. See more information about this command on [Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/core/tools/global-tools#install-a-local-tool).

The same command should be performed in build servers when [SpecSync is integrated to the CI pipeline](../important-concepts/synchronizing-test-cases-from-build.md).

```bash
dotnet tool restore
```

{% hint style="warning" %}
For a special automated Test Case scenario you might need to install an additional NuGet package to your project. This is usually not necessary even for .NET projects. For details please check the [setup instructions](setup-and-configure.md#setup-specflow-plugin).
{% endhint %}

