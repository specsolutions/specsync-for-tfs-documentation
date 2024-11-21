# Installation & Setup

{% hint style="warning" %}
Installation options have been changed in SpecSync v3. For older releases check the [Older versions](../reference/older-versions.md) reference page.
{% endhint %}

SpecSync for Azure DevOps can be installed in different ways depending on the project context. The following table contains an overview of the installation options. The detailed installation instructions can be found by clicking the name of the option.

| Option | Supported Platforms | Prerequisites | Notes |
| ------ | ------------------- | ------------- | ----- |
| [.NET tool](dotnet-core-tool.md) (NuGet, Download) | Windows, Linux, macOS, Docker | .NET Core 3.1 SDK or later (including .NET 6, 7, 8, 9) | Recommended |
| [.NET Console App](dotnet-console.md) (NuGet, Download) | Windows | .NET Framework 4.6.2 or later | Compatible with SpecSync v2.1 usage |
| [Native binaries for Linux or macOS](native-binaries.md) (Download) | Linux, macOS | - | Larger package (\~35MB) |
| [Official SpecSync Docker image](docker-image.md) (Docker Hub) | Windows, Linux, macOS | Docker (Linux containers) | Ready-to-run images that require no installation |

{% embed url="https://youtu.be/czUI-geOYmY" %}
Video tutorial about installing SpecSync as a .NET tool
{% endembed %}

Once SpecSync has been installed, you need to setup and configure it before the first use. Check the [Setup and Configure](setup-and-configure.md) page for the details.

The [Command line reference](../reference/command-line-reference/) page contains all available commands and options you can perform with SpecSync and the [Configuration](../reference/configuration/) page contains the reference documentation for the SpecSync configuration file.

Please also review all [synchronization and test result publishing features](../features/) as well.

{% content-ref url="setup-and-configure.md" %}
[setup-and-configure.md](setup-and-configure.md)
{% endcontent-ref %}

{% content-ref url="../reference/command-line-reference/" %}
[command-line-reference](../reference/command-line-reference/)
{% endcontent-ref %}

{% content-ref url="../reference/configuration/" %}
[configuration](../reference/configuration/)
{% endcontent-ref %}

{% content-ref url="../features/" %}
[features](../features/)
{% endcontent-ref %}

{% hint style="info" %}
For setting up SpecSync for for a working example project, you can also check the [Getting started](../getting-started/) guide.
{% endhint %}







