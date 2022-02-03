# local

This configuration section contains settings for the local repository \(file system\) containing the feature files.

The following example shows the available options within this section.

```javascript
{
  ...
  "local": {
    "featureFileSource": {
      "type": "projectFile",
      "filePath": "MyProject.Specs\\MyProject.Specs.csproj"
    },

    "tags": "@done and not (@ignored or @planned)",

    "sourceFiles": [
      "Folder1/",
      "Folder3/**/alpha*.feature"
    ],
    
    "defaultFeatureLanguage": "en-US"
  },
  ...
}
```

## Settings

<table>
  <thead>
    <tr>
      <th style="text-align:left">Setting</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>featureFileSource</code>
      </td>
      <td style="text-align:left">The feature file source configuration.</td>
      <td style="text-align:left">Detect project file in the folder of the configuration file</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>featureFileSource/type</code>
      </td>
      <td style="text-align:left">
        <p>The type of the feature file source configuration. Available options: <code>projectFile</code>, <code>folder</code>, <code>listFile</code> and <code>stdIn</code>.</p>
        <ul>
          <li><code>projectFile</code> &#x2014; Loads feature file list from a .NET C#
            project file (<code>.csproj</code>). SpecSync detects the project file
            by extension in the folder of the configuration file by default, but the
            project file path can also be specified in the <code>local/featureFileSource/filePath</code> setting.</li>
          <li><code>folder</code> &#x2014; Loads the feature files from a particular
            folder and its sub-folders. The folder can be specified in the <code>featureFileSource/folder</code> setting.</li>
          <li><code>listFile</code> &#x2014; <i>Deprecated: This setting will be removed in a future version. It is recommended to use <code>folder</code> value in combination with <code>sourceFiles</code> setting instead.</i> Loads the feature file list from a text
            file. Each line of the text file should contain the path of a feature file.
            Empty lines and lines start with <code>#</code> are ignored. The feature
            file path can be absolute or a relative path to the config file folder.</li>
          <li><code>stdIn</code> &#x2014; <i>Deprecated: This setting will be removed in a future version. It is recommended to use <code>folder</code> value in combination with <code>sourceFiles</code> setting instead.</i> Loads the feature file list from the standard
            input stream. The content of the input stream are handled in the same was
            as the <code>listFile</code> option. </li>
        </ul>
      </td>
      <td style="text-align:left">
        <p></p>
        <p><code>projectFile</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>featureFileSource/filePath</code>
      </td>
      <td style="text-align:left">The path of the feature file source file. Can contain an absolute or a
        relative path to the config file folder. It may contain environment variables
        in <code>...%MYENV%...</code> form.</td>
      <td style="text-align:left">mandatory for types <code>projectFile</code>, <code>listFile</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>featureFileSource/folder</code>
      </td>
      <td style="text-align:left">The folder to search the feature files in when <code>type</code> was set
        to <code>folder</code>. Can contain an absolute or a relative path to the
        config file folder. It may contain environment variables in <code>...%MYENV%...</code> form.</td>
      <td
      style="text-align:left">load feature files from the folder of the config file</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>tags</code>
      </td>
      <td style="text-align:left">A <a href="http://speclink.me/tagexpressions">tag expression</a> of scenarios
        that should be included in synchronization (e.g. <code>not @ignore</code> or <code>@done and not (@ignored or @planned)</code>).
        See <a href="../../features/common-synchronization-features/excluding-scenarios-from-synchronization.md">Excluding scenarios from synchronization</a> for
        details.</td>
      <td style="text-align:left">all scenarios included</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>sourceFiles</code>
      </td>
      <td style="text-align:left">An array of <a href="https://en.wikipedia.org/wiki/Glob_%28programming%29">glob pattern</a> of feature files that should be included in synchronization (e.g. <code>Folder1/</code> or <code>Folder3/**/alpha*.feature</code>).
        See <a href="../../features/common-synchronization-features/excluding-scenarios-from-synchronization.md">Excluding scenarios from synchronization</a> for
        details.</td>
      <td style="text-align:left">all scenarios included</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>defaultFeatureLanguage</code>
      </td>
      <td style="text-align:left">The default feature file language, e.g. <code>de-AT</code>.</td>
      <td style="text-align:left">get from SpecFlow config or use <code>en-US</code>
      </td>
    </tr>
  </tbody>
</table>

## Example: Synchronize feature files of a SpecFlow project

The SpecFlow project can be detected in the folder of the configuration file usually, in this case **no additional configuration is required**. \(SpecSync tries to find a `.csproj` file in the folder.\) In case there are multiple .NET project in the folder or the configuration file is not stored in the project root, you should configure SpecSync as below:

```javascript
{
  ...
  "local": {
    "featureFileSource": {
      "type": "projectFile",
      "filePath": "MyProject\\MyProjectFile.csproj"
    }
  },
  ...
}
```

## Example: Synchronize feature files from the `features` folder

For Cucumber-based projects, it is common to store the feature files in a folder called `features`. In order to synchronize the feature files with this setup, the feature file source has to be configured to `folder` and the required folder path has to be specified in the `folder` setting:

```javascript
{
  ...
  "local": {
    "featureFileSource": {
      "type": "folder",
      "folder": "features"
    }
  },
  ...
}
```

You can invoke the synchronization as usual:

```text
path-to-specsync-package/tools/SpecSync4AzureDevOps.exe push
```

## Example: Synchronize specific feature files

The following example synchronizes a specific set of feature files. 

Let's imagine a folder structure as the following:

```text
features/
features/feature_a.feature
features/group_a/feature_b.feature
features/group_a/feature_c.feature
features/group_a/area_1/feature_d.feature
features/group_b/feature_e.feature
```

In this example SpecSync is configured to synchronize all feature files that are:

* within `features/group_b` \(so currently `feature_e.feature`\)
* have a name that starts with `feature_d` \(so currently `features/group_a/area_1/feature_d.feature`\)

```javascript
{
  ...
  "local": {
    "featureFileSource": {
      "type": "folder"
    },
    "sourceFiles": [
      "features/group_b/",
      "features/**/feature_d*.feature"
    ]
  },
  ...
}
```

You can invoke the synchronization as usual:

```text
path-to-specsync-package/tools/SpecSync4AzureDevOps.exe push
```

{% page-ref page="./" %}

