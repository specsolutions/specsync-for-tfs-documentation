# Install as .NET tool

{% hint style="info" %}
This install method is available for machines with .NET SDK 6 or higher (including .NET 7, .NET 8, .NET 9) installed regardless of whether the BDD project uses .NET or whether it uses .NET Core. Other [installation options](./) available.
{% endhint %}

The most convenient way to use SpecSync is to install it as a .NET tool. While .NET tools require .NET 6, 7, 8 SDK or .NET 9 SDK to be installed, that can be installed to any platforms, including Linux and macOS. It also supports execution in Docker containers. .NET 9 SDK can be installed from the [.NET Download](https://dotnet.microsoft.com/download) page. We recommend installing the latest .NET SDK.

{% hint style="warning" %}
The .NET 6 and .NET 7 frameworks are out of support and will [not receive security updates in the future](https://aka.ms/dotnet-core-support). SpecSync versions v6 will not run with .NET 6 and .NET 7. Please use SpecSync with .NET 8 or any of the other supported platforms.
{% endhint %}

SpecSync can be installed as .NET tool, even if the project does not use .NET (e.g. uses Cucumber Java) or if it uses a different version of .NET (e.g. .NET Framework v4.7).

SpecSync should be installed as a _local_ .NET tool (.NET tools can also be installed globally for the machine, but that is more suitable for general, non project specific tools). By installing as local tool, you can use different SpecSync versions for different projects and the required SpecSync version is registered in the project repository. You can read more about .NET local tools on [Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/core/tools/global-tools#install-a-local-tool).

{% embed url="https://youtu.be/czUI-geOYmY" %}
Video tutorial about installing SpecSync as a .NET tool
{% endembed %}

### Step 1: Initialize .NET local tool configuration (if needed)

If you haven't used any .NET local tool in your project, you need to create the necessary configuration file. Otherwise this step can be skipped.

For initializing the configuration files, you need to run the `dotnet new tool-manifest` command from the solution or repository root directory.

```bash
dotnet new tool-manifest
```

This command creates a manifest file named `dotnet-tools.json` under the `.config` directory. This file should be added to source control so that all other members of the team can use the same tools.

### Step 2: Install SpecSync as a .NET local tool

Once the .NET local tool configuration is initialized SpecSync can be easily installed using the `dotnet tool install` command. This will download and install the latest version of SpecSync from NuGet.org.

```bash
dotnet tool install SpecSync.AzureDevOps
```

{% hint style="warning" %}
In case there is a custom NuGet source configured in your organization that does not mirror SpecSync packages, you might receive an error. In that case specifying the NuGet package source might help.

You can specify the package source by adding the `--add-source  https://api.nuget.org/v3/index.json --ignore-failed-sources` command line options to the command. For further solutions for this problem, please check the [Troubleshooting guide](../contact/troubleshooting.md#nuget-package-not-found)
{% endhint %}

### Step 3: Verify installation

SpecSync is ready to run using the `dotnet specsync` command. You can test the installation by checking the installed SpecSync version.

```bash
dotnet specsync version
```

If the correct version number is displayed, you are ready to move on to setup and configure SpecSync for the first synchronization. Check the [Setup and Configure](setup-and-configure.md) page for the details.

### Step 4: Restore SpecSync for other people working with your project

To be able to use the .NET local tools you have installed by other members of the same project, they need to "restore" the installed tools. This can be performed using the `dotnet tool restore` command, that restores all .NET local tools of the project. See more information about this command on [Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/core/tools/global-tools#install-a-local-tool).

The same command should be performed in build servers when [SpecSync is integrated to the CI pipeline](../important-concepts/synchronizing-test-cases-from-build.md).

```bash
dotnet tool restore
```

{% hint style="warning" %}
For a special automated Test Case scenario you might need to install an additional NuGet package to your project. This is usually not necessary even for .NET projects. For details please check the [setup instructions](setup-and-configure.md#setup-specflow-plugin).
{% endhint %}

## Upgrading SpecSync .NET tool

There are several ways to upgrade SpecSync if it was installed as a .NET tool. The most simple way is to use the `dotnet tool update` command. See more information about this command on [Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-tool-update).

The following command upgrades SpecSync to the latest release version.

```bash
dotnet tool update SpecSync.AzureDevOps
```

You can also upgrade SpecSync to a specific version using the --version option.

```bash
dotnet tool update SpecSync.AzureDevOps --version 3.3.8
```

{% hint style="info" %}
To be able to upgrade to a preliminary version, you always need to specify the exact version number explicitly using the --version option.
{% endhint %}
