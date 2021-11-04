# Install as .NET Console App

{% hint style="info" %}
This install method is available for Windows machines with .NET Framework v4.6.2 or higher. Other [installation options](./) available.
{% endhint %}

SpecSync can be used as a .NET Framework Windows Console App (`SpecSync4AzureDevOps.exe`). The tool can be downloaded from NuGet.org (for .NET projects) or as a downloadable zip package for other development platforms.&#x20;

## Installing SpecSync .NET Console App from NuGet for .NET projects

The SpecSync .NET Console App is wrapped as a NuGet package, [SpecSync.AzureDevOps.Console](https://www.nuget.org/packages/SpecSync.AzureDevOps.Console). The Console App is located in the `tools` folder of the package.

### Step 1: Install package

Installing it to a .NET project is similar to other NuGet packages. You can use the NuGet package manager in Visual Studio or the package management console.

```
Install-Package SpecSync.AzureDevOps.Console
```

{% hint style="info" %}
Since the NuGet package does not contain any library that would be referenced from the project, the package does not have a dependency on the .NET Framework used by the project.
{% endhint %}

Depending on your project setup, the package has been downloaded and extracted to

* into the `packages` folder of your solution: \<SOLUTION‑FOLDER>\packages\SpecSync.AzureDevOps.Console.\<SPECSYNC‑VERSION>
* into the global packages folder: C:\Users\\\<USER>\\.nuget\packages\specsync.azuredevops.console\\\<SPECSYNC‑VERSION>
* into an other folder configured in the `NuGet.config` file

### Step 2: Create/review a CMD file in order to execute the console app easily from the installation folder (optional)

As the package installation folder is pretty complicated to type-in frequently, it is recommended to add a simple command line shell script (`specsync4azuredevops.cmd`) that executes the tool from the right folder.&#x20;

{% hint style="info" %}
In case you use NuGet packages from the `packages` folder of your solution and your project uses the classic project file format, the package installation populated a `specsync4azuredevops.cmd `into the project folder. Review the script and apply any changes if necessary.
{% endhint %}

A `specsync4azuredevops.cmd` script that executes the SpecSync .NET Console App from the local packages folder might look similar to this:

```
@REM Executing SpecSync .NET Console App by forwarding all command line parameters
@REM Note: the version number has to be updated after a SpecSync version upgrade
%~dp0\..\packages\SpecSync.AzureDevOps.Console.3.0.0\tools\SpecSync4AzureDevOps.exe %*

```

A `specsync4azuredevops.cmd` script that executes the SpecSync .NET Console App from the global packages folder might look similar to this:

```
@REM Executing SpecSync .NET Console App by forwarding all command line parameters
@REM Note: the version number has to be updated after a SpecSync version upgrade
%HOMEPATH%\.nuget\packages\SpecSync.AzureDevOps.Console\3.0.0\tools\SpecSync4AzureDevOps.exe %*

```

### Step 3: Verify installation

SpecSync is ready to run using the `specsync4azuredevops.cmd` script. You can test the installation by checking the installed SpecSync version.

```bash
specsync4azuredevops.cmd version
```

If the correct version number is displayed, you are ready to move on to setup and configure SpecSync for the first synchronization. Check the [Setup and Configure](setup-and-configure.md) page for the details.

{% hint style="warning" %}
For a special automated Test Case scenario you might need to install an additional NuGet package to your project. This is usually not necessary even for .NET projects. For details please check the [setup instructions](setup-and-configure.md#setup-specflow-plugin).
{% endhint %}

## Installing SpecSync .NET Console App for other development platforms

The SpecSync .NET Console App can be downloaded directly as a zip file from our website. The download URLs can be found on the [Downloads](../downloads.md) page.

After extracting the package, the `SpecSync4AzureDevOps.exe` executable can be found in the `tools` folder.

### Verify installation

You can test the installation by checking the installed SpecSync version. Open a command prompt from your project folder and invoke the following command.

```bash
<EXTRACTED-SPECSYNC-FOLDER>\tools\specsync4azuredevops.exe version
```

If the correct version number is displayed, you are ready to move on to setup and configure SpecSync for the first synchronization. Check the [Setup and Configure](setup-and-configure.md) page for the details.
