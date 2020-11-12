# Installation & Setup

{% hint style="warning" %}
Installation options have been changed in SpecSync v3. For older releases check the [Older versions](../reference/older-versions.md) reference page.
{% endhint %}

SpecSync for Azure DevOps can be installed in different ways depending on the project context. The following table contains an overview of the installation options. The detailed installation instructions can be found by clicking the name of the option.

| Option | Supported Platforms | Prerequisites | Notes |
| :--- | :--- | :--- | :--- |
| [.NET Core tool](dotnet-core-tool.md) \(NuGet, Download\) | Windows, Linux, macOS, Docker | .NET Core 3.0 SDK or later | Recommended |
| [.NET Console App](dotnet-console.md) \(NuGet, Download\) | Windows | .NET Framework 4.6.2 or later | Compatible with SpecSync v2.1 usage |
| [Native binaries for Linux or macOS](native-binaries.md) \(Download\) | Linux, macOS | - | Larger package \(~35MB\) |
| [Official SpecSync Docker image](docker-image.md) \(Docker Hub\) | Windows, Linux, macOS | Docker \(Linux containers\) | Ready-to-run images that require no installation |

{% embed url="https://youtu.be/qrfSX\_pXyNA" caption="Video tutorial about installing SpecSync as a .NET Core tool" %}

Once SpecSync has been installed, you need to setup and configure it before the first use. Check the [Setup and Configure](setup-and-configure.md) page for the details. 

The [Command line reference](../reference/command-line-reference/) page contains all available commands and options you can perform with SpecSync and the [Configuration](../reference/configuration/) page contains the reference documentation for the SpecSync configuration file.

Please also review all [synchronization and test result publishing features](../features/) as well.

{% page-ref page="setup-and-configure.md" %}

{% page-ref page="../reference/command-line-reference/" %}

{% page-ref page="../reference/configuration/" %}

{% page-ref page="../features/" %}

{% hint style="info" %}
For setting up SpecSync for for a working example project, you can also check the [Getting started](../getting-started/) guide.
{% endhint %}









