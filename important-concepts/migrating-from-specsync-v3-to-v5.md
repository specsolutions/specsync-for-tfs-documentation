# Migrating from SpecSync v3 to v5

{% hint style="success" %}
The licensing model of SpecSync allows you to use any versions, so it is recommended to always upgrade to the latest version.
{% endhint %}

{% hint style="hint" %}
SpecSync changed to use semantic versioning from v5 on. This means that the changes that earlier were minor version changes (e.g. v3.3 to v3.4) are now represented with major version changes. The version that follows v3.4 was earlier planned as v3.5 (or v1.4 in case of SpecSync for Jira) but promoted now to v5.
{% endhint %}

SpecSync for Azure DevOps v5 has introduced a couple of useful improvements but the features and the configuration is highly backwards compatible with the v3 releases, therefore the migration process is usually simple.

You can find a complete list of improvements in the [Changelog](../changelog.md#v5).

To perform the version upgrade, please follow the steps below.

### Step 1: Verify compatibility

Check the minimum [supported Azure DevOps server version](../reference/compatibility.md#supported-server-systems). There are a few older Azure DevOps server versions that are not supported by v5. In case you need to synchronize scenarios with any of these you cannot upgrade to v5. As these sever version are not supported by Microsoft anymore, we recommend upgrading to a newer version.

Server versions not supported by v5 anymore:
  * Team Foundation Server 2017 (Microsoft support ended: [January, 2022](https://learn.microsoft.com/en-us/lifecycle/products/visual-studio-team-foundation-server-2017))
  * Team Foundation Server 2018 (Microsoft support ended: [January, 2023](https://learn.microsoft.com/en-us/lifecycle/products/visual-studio-team-foundation-server-2018))
  * Azure DevOps Server 2019 (Microsoft support ended: [April, 2024](https://learn.microsoft.com/en-us/lifecycle/products/azure-devops-server-2019))

Some older .NET versions are not supported anymore. Please note that the .NET version requirement is only about the .NET SDK installed in order to run SpecSync. The synchronize project can still target old .NET versions as well.

If you cannot install a compatible .NET version to your systems where SpecSync is executed, we recommend using the SpecSync [binary installation](../installation/native-binaries.md) or execute SpecSync from [Docker image](../installation/docker-image.md).

.NET versions not supported by v5 anymore:
* .NET 3.1
* .NET 5

### Step 2: Review breaking changes

Besides the server versions or .NET frameworks that are not supported anymore, there are also a few minor breaking changes as well. 

These are partially related to removing features that have been marked deprecated earlier. The features that have been deprecated always provide a warning, so if your SpecSync v3.4 commands currently execute without warnings, these will not impact your usage.

The remaining breaking changes do not impact the most of usages, but please review the complete list of [breaking changes](../changelog.md#v5-breaking)

### Step 3: Learn about new concepts

SpecSync v5 has introduced some new useful features and concepts. These are not mandatory to use, but they can improve your synchronization experience. For the best migration experience, we recommend learning about these new features before performing the upgrade.

Some of the most important new concepts are:
- [Remote scope concept](../features/common-synchronization-features/remote-scope.md) that allows better control of which work items are synchronized and allows tracking work items that have been deleted from the source. The concept was used already in the previous versions, but in v5 there are more and better ways to utilize it.
- [Configuration keys concept](../features/common-synchronization-features/configuration-key.md) that can help to track which work items belong to which synchronization configuration in case you have multiple configurations synchronizing to the same project.
- [Hierarchies feature](../features/common-synchronization-features/synchronizing-test-case-hierarchies.md) that allows better control of how Test Cases are organized in Azure DevOps Test Suites. This feature replaces the earlier deprecated `addTestCasesToSuites` customization.
- License keys provide a new way for using your SpecSync license. Instead of providing the license file, you can configure the license key in the configuration file or in an environment variable. Please check the [Licensing documentation](../licensing.md#license-keys) for more details.

### Step 4: Upgrade SpecSync

Upgrade SpecSync to the latest v5 version. Please check the [How to upgrade to a newer version of SpecSync](how-to-upgrade-specsync.md) guide for detailed steps.

### Step 5: Perform SpecSync upgrade command

One of the new features of SpecSync v5 is the `upgrade` command. This command can be used to invoke the upgrade wizard that automatically fixes changes in the configuration file (e.g. for renamed configuration settings) and also prompts for configuring additional new features or configuration options.

```text
dotnet specsync upgrade
```

The `upgrade` command only modifies the configuration file but it does not change anything in the Azure DevOps server. So you can perform the `upgrade` command safely and review the changes in the configuration file manually before performing a synchronization.

{% hint style="hint" %}
You do not have to configure all optional feature with the `upgrade` command in one step. You can of course configure these features also manually by modifying the configuration file, but you can also re-run the `upgrade` command later to configure these.
{% endhint %}

### Step 6: Review configuration changes and perform synchronization

Once the `upgrade` command has been executed, you can review the changes of the configuration file.

If the changes look good, you can perform a synchronization with the `push` command. It is recommended first to review the synchronization steps without actually changing the Azure DevOps server using the `--dryRun` option.

```text
dotnet specsync push --dryRun
```

If the result is good, you can do the final synchronization with the `push` command.

```text
dotnet specsync push
```

### Step 7: Review Azure DevOps pipeline usage

SpecSync v5 has improved integration with Azure DevOps pipelines. The most important change is that the task will not fail automatically on warnings, but will register the warning to the pipeline result instead. Therefore the option `--zeroExitCodeForWarnings` is not needed anymore.

If you want to fail the pipelines for warnings, use the `--treatWarningsAsErrors` option.
