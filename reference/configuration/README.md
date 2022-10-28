# Configuration reference

SpecSync can be configured using a json configuration file, by default called `specsync.json`. This section contains a detailed reference of the SpecSync configuration options. For a general introduction about how SpecSync configuration works, please check the [Configuration file](../../features/general-features/configuration-file.md) page. 

The [Setup and Configure](../../installation/setup-and-configure.md) page contains information about how to initialize the configuration file.

{% hint style="info" %}
Most IDE editors \(e.g. Visual Studio, Atom, Visual Studio Code\) provide auto-completion and in-place documentation for SpecSync configuration files. Please make sure you keep the `"$schema"` setting at the top of the file.
{% endhint %}

## A minimal configuration file

The following example shows a minimal configuration file.

```javascript
{
  "$schema": "http://schemas.specsolutions.eu/specsync4azuredevops-config-latest.json",
  "compatibilityVersion": 3.3,

  "remote": {
    "projectUrl": "https://specsyncdemo.visualstudio.com/MyCalculator",
  }
}
```


{% hint style="info" %}
The `compatibilityVersion` setting at the top of the file specifies the SpecSync version that the configuration file was created for. The new SpecSync features can be used even if the compatibility version is older than the current version, but if the default values of some settings change in the future, SpecSync will not apply these default unless the compatibility version is updated.
{% endhint %}

## Configuration sections

The settings in the configuration file are grouped into different configuration sections. Each configuration section is a JSON object following the syntax:

```javascript
{  // start of the file
  ...
  "section1": {
    // settings for section 'section1' 
  },
  ...  
}  // end of the file
```

_Note: The leading comma \(_`,`_\) after the curly brace close \(_`}`_\) of_ `section1` _is not needed if this is the last section in the file._

{% hint style="info" %}
The SpecSync configuration file supports `//` comments.
{% endhint %}

## Available configuration sections

The following configuration sections can be used. Click to the name of the section for detailed documentation.

* [`toolSettings`](configuration-toolsettings.md) — settings for the synchronization tool
* [`local`](configuration-local.md) — settings for the local repository \(file system\) containing the feature files
* [`remote`](configuration-remote.md) — Settings for accessing the test cases on the remote Azure DevOps server
* [`knownRemotes`](configuration-remote.md) — Specify connection and authentication details of Azure DevOps projects that are used by multiple synchronization projects
* [`synchronization`](configuration-synchronization/) — synchronization settings
* [`specFlow`](configuration-specflow.md) — settings related to synchronizing SpecFlow projects
* [`customizations`](configuration-customizations.md) — configure customizations

