# Hierarchical configuration files

SpecSync can compose the necessary configuration settings from multiple configuration files. Using multiple configuration files are useful to separate user-specific settings, like credentials \([User-specific configuration files](hierarchical-configuration-files.md#user-specific-configuration-files)\) or when the project requires multiple synchronization configurations without duplicating the common settings \([Parent configuration files](hierarchical-configuration-files.md#parent-configuration-files)\).

## User-specific configuration files

SpecSync requires to connect to the Azure DevOps server. In many cases the authentication credentials are user-specific and therefore they cannot be included into the project-specific SpecSync configuration files \(because those are typically added to source-control and shared with other team members\).

These settings can be specified in the user-specific SpecSync configuration file. The user-specific configuration file is stored in the local application data folder of the current user. See locations on the different operating systems below. 

{% tabs %}
{% tab title="Windows" %}
`C:\Users\<username>\AppData\Local\SpecSync\specsync.json`
{% endtab %}

{% tab title="macOS" %}
`/Users/<username>/.local/share/SpecSync/specsync.json`
{% endtab %}

{% tab title="Linux" %}
`$HOME/.local/share/SpecSync/specsync.json`
{% endtab %}
{% endtabs %}

User-specific configuration files are created automatically when you ask to save the credentials during the [init command](../../reference/command-line-reference/init-command.md) or during interactive authentication. Alternatively the files can also be created manually using the examples below.

User-specific configuration files can be used to specify configurations of [`toolSettings`](../../reference/configuration/configuration-toolsettings.md) and [`knownRemotes`](../../reference/configuration/configuration-knownremotes.md) configuration sections.

To store the authentication credentials for the different Azure DevOps projects, the [knownRemotes setting](../../reference/configuration/configuration-knownremotes.md) can be specified.

You can also copy the SpecSync license file to the same folder and specify the default license file location in the user-specific configuration file using the [toolSettings/licensePath](../../reference/configuration/configuration-toolsettings.md) setting.

### Example

The following example shows a user-specific configuration file that specifies an authentication credential for all projects within an Azure DevOps organization and another one for a specific project. It also specifies the default location of the license file.

{% code title="specsync.json" %}
```javascript
{
  "$schema": "https://schemas.specsolutions.eu/specsync-user-config-latest.json",

  "toolSettings": {
    "licensePath": "specsync.lic" // in the same folder as the user-specific setting
  },
  "knownRemotes": [
    {
      "projectUrl": "https://dev.azure.com/myorg/*",
      "user": "g2x.....................ac2vx57i4a"
    },
    {
      "projectUrl": "https://dev.azure.com/otherorg/OtherProject",
      "user": "y6s.....................ksuc7tsts"
    }
  ]
}
```
{% endcode %}

## Parent configuration files

In bigger projects you might need to create multiple synchronization configurations \(e.g. different parts of the project has to be synchronized to different Test Suites or the synchronization on the CI pipeline requires different settings\).

To support these cases, SpecSync can load the common settings from parent configuration files. SpecSync considers any `specsync.json` file in one of the parent folders of the current configuration file as parent, but the parent configuration file can also be specified explicitly using the [toolSettings/parentConfig](../../reference/configuration/configuration-toolsettings.md) setting.

Loading parent configuration files can be disabled using the [toolSettings/ignoreParentConfig](../../reference/configuration/configuration-toolsettings.md) setting.

### Example: Project with different feature file sets

If the project has multiple feature-file sets \(folders containing feature files, e.g. multiple SpecFlow projects\), you can create a specsync.json file in the repository root and override the default settings with configuration files in the specific feature-file set. So the folder and configuration hierarchy would look like this:

```javascript
repository-root/
  area-a/
    features/
      f1.feature
      f2.feature
    specync.json // specific settings for area "A"
  area-b/    
    features/
      f3.feature
      f4.feature
    specync.json // specific settings for area "B"
  specync.json // common settings
```

### Example: Project with different synchronization settings

If the project requires different synchronization settings for the CI pipeline, the default settings can be overwritten using a specific configuration file that uses the default configuration file as parent. The specific configuration file could look like this:

{% code title="specsync-ci.json" %}
```javascript
{
  "$schema": "https://schemas.specsolutions.eu/specsync4azuredevops-config-v5-latest.json",

  "toolSettings": {
    "parentConfig": "specsync.json", // default config file in the same folder
  },
  "synchronization": {
    "disableLocalChanges": true  // overridden setting
  }
}
```
{% endcode %}

