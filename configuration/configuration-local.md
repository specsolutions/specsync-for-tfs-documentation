# local

This configuration section contains settings for the local repository \(file system\) containing the feature files.

The following example shows the available options within this section.

```text
{
  ...
  "local": {
    "featureFileSource": {
      "type": "projectFile",
      "filePath": "MyProject.Specs\\MyProject.Specs.csproj"
    },

    "tags": "@done and not (@ignored or @planned)"
  },
  ...
}
```

## Settings

* `featureFileSource` -- The feature file source configuration. \(Default: _\[Detect project file in the folder of the configuration file\]_\) 
  * `featureFileSource/type` -- The type of the feature file source configuration. Available options: `projectFile`, `folder`, `listFile` and `stdIn`.
    * `projectFile` -- Loads feature file list from a .NET C\# project file \(`.csproj`\). SpecSync detects the project file by extension in the folder of the configuration file by default, but the project file path can also be specified in the `local/featureFileSource/filePath` setting.
    * `folder` -- Loads the feature files from a particular folder and its sub-folders. The folder can be specified in the `featureFileSource/folder` setting.
    * `listFile` -- Loads the feature file list from a text file. Each line of the text file should contain the path of a feature file. Empty lines and lines start with `#` are ignored. The feature file path can be absolute or a relative path to the config file folder. See example below and [Getting started using Cucumber](../getting-started/getting-started-cucumber.md) for more details.
    * `stdIn` -- Loads the feature file list from the standard input stream. The content of the input stream are handled in the same was as the `listFile` option. See example below and [Getting started using Cucumber](../getting-started/getting-started-cucumber.md) for more details. 
  * `featureFileSource/filePath` -- The path of the feature file source file. Can contain an absolute or a relative path to the config file folder. It may contain environment variables in `...%MYENV%...` form.
  * `featureFileSource/folder` -- The folder to search the feature files in when `type` was set to `folder`. Can contain an absolute or a relative path to the config file folder. It may contain environment variables in `...%MYENV%...` form. \(Default: _\[load feature files from the folder of the config file\]_\)
* `tags` -- A [tag expression](http://speclink.me/tagexpressions) of scenarios that should be included in synchronization \(e.g. `not @ignore` or `@done and not (@ignored or @planned)`\). See [Filters and scopes](../important-concepts/filters-and-scopes.md) for details. \(Default: _\[all scenarios included\]_\)

## Example: Synchronize specific feature files

The following example synchronizes a specific set of feature files.

We need a text file with the list of feature files to be synchronized, let's call it `specsync-features.txt`. Save it in the project root folder, where the `specsync.json` configuration file is located.

```text
features/addition.feature
features/special/complex_addition.feature
# this line is ignored
features/multiplication.feature
```

All paths in this file can be relative to the folder of the config file. On Windows platform the `\` character has to be used instead of the `/`.

The list file has to be specified in the config file:

```text
{
  ...
  "local": {
    "featureFileSource": {
      "type": "listFile",
      "filePath": "specsync-features.txt"
    }
  },
  ...
}
```

You can invoke the syncronization as usual:

```text
path-to-specsync-package/tools/SpecSync4AzureDevOps.exe push
```

## Example: Synchronize feature files from a sub-folder

Let's imagine a folder structure as the following:

```text
features/
features/feature_a.feature
features/group_a/feature_b.feature
features/group_a/feature_c.feature
features/group_a/area_1/feature_d.feature
features/group_b/feature_e.feature
```

In this example SpecSync is configured to synchronize all feature files within `features/group_a` \(so currently `feature_b.feature`, `feature_c.feature`, `feature_d.feature`\), but without listing them explicitly in a file.

For this, the feature file source has to be configured to `folder` and the required folder path has to be specified in the `folder` setting:

```text
{
  ...
  "local": {
    "featureFileSource": {
      "type": "folder",
      "folder": "features/group_a"
    }
  },
  ...
}
```

You can invoke the syncronization as usual:

```text
path-to-specsync-package/tools/SpecSync4AzureDevOps.exe push
```

## Example: Synchronize feature files from a dynamic folder list

In this example we will synchronize the same feature files as in the previous example \(all feature files within `features/group_a`\), but now the list of feature file is provided by an external tool and SpecSync will receive it through through the standard input stream with piping.

First, let's set the config file to:

```text
{
  ...
  "local": {
    "featureFileSource": {
      "type": "stdIn"
    }
  },
  ...
}
```

After that we can generate the file list and invoke the synchronization. Let's suppose the current folder is the project root.

On Windows \(CMD\):

```text
dir /s /b features\group_a\*.feature | path-to-specsync-package\tools\SpecSync4AzureDevOps.exe push
```

On Windows \(PowerShell\):

```text
gci -Path .\features\group_a -r *.Feature | % FullName | path-to-specsync-package\tools\SpecSync4AzureDevOps.exe push
```

On OSX and Linux:

```text
find features/group_a/ -name *.feature | path-to-specsync-package/tools/SpecSync4AzureDevOps.exe push
```

_\[Back to the_ [_Configuration guide_](./)_\]_

