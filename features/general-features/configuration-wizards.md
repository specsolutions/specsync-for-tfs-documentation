# Configuration wizards

SpecSync provides configuration wizard commands that can help creating or modifying the [SpecSync configuration files](configuration-file.md).

The wizards can be invoked from and interactive console like the other command line tool and they go through several steps to prepare the configuration change. The prepared configuration changes are summarized as the last wizard step and the user can confirm or reject the changes. 

You can cancel the configuration wizards at any step by pressing *Ctrl+C*.

During the wizard execution, you might be prompted for specifying different configuration values or choices. For those prompts where there is a meaningful default answer, the default is displayed in parenthesis (e.g. `Do you want to check connection? (yes)`). When pressing the *Enter* key without specifying any value, the default value will be used.

The sections below describe the available wizards.

## Init wizard

The `init` command initializes SpecSync configuration by creating a `specsync.json` configuration file based on the settings provided for the interactive questions asked by the command.

```text
dotnet specsync init
```

Please find the available command line options at the [`init` command](../../reference/command-line-reference/init-command.md) reference page.

It is recommended to invoke the `init` command from the common root folder of the local test cases (feature files) to be synchronized.

During the execution of the command, the basic configuration settings (project URL and authorization settings) can be specified. The init command verifies the connection to the Azure DevOps project specified to avoid common authentication issues.

In case of [Personal Access Token \(PAT\) authentication](../../features/general-features/server-authentication-options.md), the command also offers you to save the authentication details to the user-specific configuration file, so that the credentials are not included in the project configuration.

The `init` command also offers configuring some useful optional features, like [remote scope](../common-synchronization-features/remote-scope.md) or [hierarchy synchronization](../common-synchronization-features/synchronizing-test-case-hierarchies.md).

## Upgrade wizard

{% hint style="warning" %}
This feature is available with SpecSync v5 or above. 
{% endhint %}

The `upgrade` command upgrades the SpecSync configuration file after a SpecSync version upgrade.

```text
dotnet specsync upgrade
```

Please find the available command line options at the [`upgrade` command](../../reference/command-line-reference/upgrade-command.md) reference page.

The command performs upgrades the configuration simple changes (e.g. renamed settings) automatically and provides options to configure optional features as well that have been introduced with the new version.

The `upgrade` command only modifies the configuration file but it does not change anything in the Azure DevOps. So you can perform the `upgrade` command safely and review the changes in the configuration file manually before performing a synchronization.

{% hint style="hint" %}
You do not have to configure all optional feature with the `upgrade` command in one step. You can of course configure these features also manually by modifying the configuration file, but you can also re-run the `upgrade` command later to configure these.
{% endhint %}

