# Using SpecSync on macOS or Linux

SpecSync can synchronize any scenarios that are written in Gherkin format. The synchronization tool works on Windows, macOS and Linux. SpecSync can also be executed in a Docker container. The page [Installation & Setup](../installation/) contains an overview of all installation options with their supported platforms.

On macOS and Linux the easiest way to install SpecSync is to download the [native binaries](../installation/native-binaries.md) or run though the [official SpecSync Docker container](../installation/docker-image.md). If the systems have .NET Core installed, installing SpecSync as a [.NET Core tool](../installation/dotnet-core-tool.md) is the easiest. You can also [build your own Docker container](using-specsync-inside-a-docker-container.md) to host SpecSync.

Regardless of the execution platform, the synchronization settings have to be specified in a configuration file \(`specsync.json`\). Typically the [`remote`](../reference/configuration/configuration-remote.md) and [`local`](../reference/configuration/configuration-local.md) configuration sections have to be specified. Please check the the [Setup and Configure](../installation/setup-and-configure.md) page and the [Getting started using Cucumber or other Gherkin-based BDD tool](../getting-started/getting-started-cucumber.md) pages for details.

