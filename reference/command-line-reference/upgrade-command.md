# upgrade

Upgrades the SpecSync configuration file after a SpecSync version upgrade.

The command performs upgrades the configuration simple changes (e.g. renamed settings) automatically and provides options to configure optional features as well that have been introduced with the new version.

The `upgrade` command only modifies the configuration file but it does not change anything in the Azure DevOps. So you can perform the `upgrade` command safely and review the changes in the configuration file manually before performing a synchronization.

{% hint style="hint" %}
You do not have to configure all optional feature with the `upgrade` command in one step. You can of course configure these features also manually by modifying the configuration file, but you can also re-run the `upgrade` command later to configure these.
{% endhint %}

See [Configuration wizards](../../features/general-features/configuration-wizards.md) for more information about the command.

## Options

| Option | Description | Default |
| :--- | :--- | :--- |
| `-c`\|`--config` &lt;CONFIG‑FILE‑PATH&gt; | The full path or the folder of the configuration file being created. | `specsync.json` in the current folder |
| `-v`, `--verbose` | If specified, error messages and trace information will be displayed more in detail. Overrides `toolSettings/outputLevel` setting of the configuration file. | false |

## Examples

Upgrade configuration by specifying all settings interactively:

```text
dotnet specsync upgrade
```

Upgrade configuration of a specific configuration file

```text
dotnet specsync upgrade -c my-specsync-config.json
```

{% page-ref page="./" %}

