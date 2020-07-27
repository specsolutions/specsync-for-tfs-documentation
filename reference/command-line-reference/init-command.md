# init

Initializes SpecSync configuration by creating a `specsync.json` configuration file based on the settings provided for the interactive questions asked by the command.

During the execution of the command, the basic configuration settings \(project URL and authorization settings\) can be specified. The init command verifies the connection to the Azure DevOps project specified to avoid common authentication issues.

In case of [Personal Access Token \(PAT\) authentication](../../important-concepts/tfs-authentication-options.md), the command also offers you to save the authentication details to the user-specific configuration file, so that the credentials are not included in the project configuration.

## Options

| Option | Description | Default |
| :--- | :--- | :--- |
| `-p`\|`--projectUrl` &lt;PROJECT‑URL&gt; | The project URL of the Azure DevOps project. | interactive prompt |
| `-c`\|`--config` &lt;CONFIG‑FILE‑PATH&gt; | The full path or the folder of the configuration file being created. | `specsync.json` in the current folder |
| `-v`, `--verbose` | If specified, error messages and trace information will be displayed more in detail. Overrides `toolSettings/outputLevel` setting of the configuration file. | false |

## Examples

Initialize configuration by specifying all settings interactively:

```text
dotnet specsync init
```

Initialize configuration for a specific project:

```text
dotnet specsync init -p https://dev.azure.com/myorganization/MyProject
```

