# Migrating from SpecSync v2 to v3

{% hint style="success" %}
The licensing model of SpecSync allows you to use any versions, so it is recommended to always upgrade to the latest version.
{% endhint %}

SpecSync for Azure DevOps v3.0 has introduced a couple of useful improvements but the features and the configuration is highly backwards compatible with the v2 releases, therefore the migration process is usually simple. 

The new version uses a newer Azure DevOps API and it also supports .NET Core. Because of the platform improvements the invocation of the synchronization tool has been changed.

You can find a complete list of improvements in the [Changelog](../changelog.md#v-3-0-0-2020-07-24).

To perform the version upgrade, please follow the steps below.

### Step 1: Verify compatibility

T**eam Foundation Server 2015 is not supported** by SpecSync v3 because of the newer Azure DevOps API it uses. In case you need to synchronize scenarios with Team Foundation Server 2015 you cannot upgrade to v3.

The minimum required .NET Framework has been changed to **.NET Framework v4.6.2** instead of v4.5. Please verify if the .NET Framework version on the machines where the synchronization tool has to be invoked is at least v4.6.2. SpecSync v3 introduced invocation options that do not require .NET Framework to be installed at all \(see [Installation & Setup](../installation/)\), so you can use SpecSync even if this criteria is not met. 

SpecFlow+ Excel support has been removed.

Linux Mono support has been removed, but there are alternative invocation options for using SpecSync v3 on Linux or macOS \(see Step 2\).

### Step 2: Review and choose tool invocation option

SpecSync can be invoked as a .NET Framework console application as before, but with v3 there are other options as well. You can run is as a .NET Core tool, as native binary or through Docker. The [Installation & Setup](../installation/) page contains a detailed overview of the different options.

As the default option \(the option provided by the main `SpecSync.AzureDevOps` NuGet package\) has been changed to invoking SpecSync as a .NET Core tool, so if you would like to use SpecSync as a .NET Console App, like in SpecSync v2.1, you need to use the **`SpecSync.AzureDevOps.Console`** NuGet package instead of `SpecSync.AzureDevOps`.  

The `SpecSync.AzureDevOps.Console` package works exactly the same way as the `SpecSync.AzureDevOps`package was working in v2.1: there is a .NET executable `SpecSync4AzureDevOps.exe` in the `tools` folder of the package. You can find more information about this option in the [Install as .NET Console App](../installation/dotnet-console.md) page. 

{% hint style="info" %}
You can upgrade to v3 and keep the .NET Console App execution and change to another invocation option later.
{% endhint %}

### Step 3: Review deprecated configuration settings and command line options

There a few rarely used configuration settings and command line options that has been deprecated or renamed in SpecSync v3. Please review the table below and perform the changes if necessary.

| Deprecated setting/option | Change it to |
| :--- | :--- |
| `--buildServerMode` command line option has been renamed | Use `--disableLocalChanges` instead. The old option can be used still, but it is going to be fully removed in one of the next versions. |
| `synchronization`/`enableLocalChanges` configuration setting has been changed | Use `synchronization`/`disableLocalChanges` instead |
| `synchronization`/`forceUpdate` configuration setting has been removed | Use `--force` command line option instead |
| `specFlow` / `useLegacyFeatureFileGenerator` configuration setting has been removed | The default \(Deveroom-based\) feature file generator supports all known cases. Please [contact support](../contact/specsync-support.md) if there are problems with it. |





