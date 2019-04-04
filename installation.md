# Installation

For a detailed installation instructions, please check the [Getting started](getting-started/) guide.

## For .NET \(SpecFlow\) projects

For .NET \(SpecFlow\) projects, SpecSync can be installed via NuGet. The synchronization tool can be installed by adding the [`SpecSync.AzureDevOps`](https://www.nuget.org/packages/SpecSync.AzureDevOps) package from NuGet.org.

```text
PM> Install-Package SpecSync.AzureDevOps
```

For synchronizing automated test cases using the "Test Suite based execution" strategy with MsTest and NUnit, an SpecFlow plugin has to be installed additionally. The name of the package depends on the SpecFlow version you use, e.g. for SpecFlow v2.3 the [`SpecSync.AzureDevOps.SpecFlow.2-3`](https://www.nuget.org/packages/SpecSync.AzureDevOps.SpecFlow.2-3) package has to be used. See more details in [Synchronizing automated test cases](important-concepts/synchronizing-automated-test-cases.md).

## For any platforms \(e.g. for Cucumber\)

SpecSync can be downloaded as a ZIP file, the file contains the synchronization tool inside the `tools` folder. The download links can be found on the [Downloads](downloads.md) page.

Prerequisites for running the synchronization tool.

* On Windows systems: .NET framework 4.5 or later
* On OSX/Linux: Mono

If you need to use SpecSync but cannot ensure the prerequisites, please contact support \(specsync@specsolutions.eu\).

