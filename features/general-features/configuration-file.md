# Configuration file

This page contains general information about the SpecSync configuration files. For a list of all configuration options, please check the [Configuration](../../reference/configuration/) reference.

## The `specsync.json` configuration file.

SpecSync can be configured using a json configuration file, by default called `specsync.json`. This file contains all information required to perform the different synchronization tasks. Some settings of the configuration file can be also overridden from the command line options of the synchronization tool, these are listed in the [Command line reference](../../reference/command-line-reference/) guide.

An initial configuration file can be generated using the SpecSync [init command](../../reference/command-line-reference/init-command.md).

The `specsync.json` configuration file is a standard JSON file, but it also allows `//` style comments. There is a JSON schema available for the configuration file that contains the available configuration options and a short description.

**It is recommended to edit the SpecSync configuration files in Visual Studio \(or other editor that supports JSON schema, like Visual Studio Code\) to get auto-completion for editing and documentation hints if you hover your mouse over a setting.**

![Code completion of the configuration file in Visual Studio](../../.gitbook/assets/configuration-completion-in-vs.png)

## Example

The following example shows a minimal configuration file.

```javascript
{
  "$schema": "http://schemas.specsolutions.eu/specsync4azuredevops-config-latest.json",

  "remote": {
    "projectUrl": "https://specsyncdemo.visualstudio.com/MyCalculator",
  }
}
```
