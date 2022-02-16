# How to define the local feature-set to be synchronized

When synchronizing the scenarios from a local repository you can define different set of scenarios to be synchronized. Maybe you want to synchronize all scenarios within the repository in one batch but maybe you need to have separate synchronization rules for the frontend and the backend scenarios. 

The feature-set to be synchronized is called the *local scope* in SpecSync and can be specified using the [`local` configuration file settings](../reference/configuration/configuration-local.md). You can learn about local and remote scopes and the difference between scopes and filers in the guide [Filters and scopes](filters-and-scopes.md).

In this guide you will find the most common scenarios and the necessary steps to do this.

## Synchronize all scenarios from a folder

If you want to synchronize all scenarios from a folder (e.g. the `features` folder of your local repository), the easiest solution is to place your SpecSync configuration file into that folder. As the default settings of the local scope configuration take all feature files from a folder (and its sub-folders), you don't even have to specify anything in the `local` configuration.

The default local scope configuration in this case is equivalent to this settings:

```javascript
{
  ...
  "local": {
    // these are the default settings for configurations in regular folders, so you don't even need to set these
    "featureFileSource": {
      "type": "folder",
      "folder": "."
    }
  },
  ...
}
```

If for some reason you don't want to put the configuration file into the folder of your feature files, you can set the folder to be used explicitly using the `local/featureFileSource/folder` setting. 

In the following example, the `specsync.json` configuration file is in the local repository root folder, but the feature files are in the `src/features` folder. 

```javascript
{
  ...
  "local": {
    "featureFileSource": {
      "type": "folder",
      "folder": "src/features"
    }
  },
  ...
}
```

{% hint style="info" %}
The path you specify in the `local/featureFileSource/folder` setting is relative to the location of the configuration file but can also contain relative paths above the current folder, e.g. `../features`.
{% endhint %}

## Synchronize all scenarios from a SpecFlow project

Although modern .NET projects include automatically all files within the project folder you can explicitly exclude some files with a .NET project. If you want to synchronize those feature files that are included in the .NET project, you simply have to put the `specsync.json` configuration file to the project root folder and do not specify anything as `local/featureFileSource`. 

Alternatively you can specify the project file explicitly:

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

## Synchronize only selected scenarios from the feature files

Regardless whether you have used the folder-based or the .NET project based approach, you can further limit the scenarios or the feature files to be applied. 

You can limit the synchronized scenarios to the ones match to a tag expression (e.g. everything tagged with `@important`) or you can specify a set of feature file paths globs, that should be included (e.g. `**/Important*.feature`). You can even combine these two options.

These settings can be configured with the `local/tags` and the `local/sourceFiles` settings. You can read more information about these options in the [Excluding scenarios from synchronization](../features/common-synchronization-features/excluding-scenarios-from-synchronization.md) page.


## Synchronizing multiple feature-sets from one local repository

If the local repository contains multiple sets of feature files that need to be synchronized separately (e.g. frontend and backend related scenarios), you can create multiple SpecSync configuration files for each of them.

In many cases some of the configuration options are common for the multiple feature-sets that you don't want to maintain separately in the configuration files of the particular feature sets. For this you can use the [hierarchical configuration files](../features/general-features/hierarchical-configuration-files.md) and specify the common settings in a parent configuration file.

For organizing the configuration files for this setup, you have two main options to choose from: the *configuration per folder* and the *different configuration files in root* option. These options are shown in the sections below.

The *different configuration files in root* option can also be used if the feature files are all in the same folder, and the scenarios are only separated by tags or file name conventions. This approach is shown in the section [Synchronize multiple feature-sets from the same folder](#multiple-feature-set-same-folder) below.

### Configuring multiple feature-sets by placing a configuration file in each folder

Let's imagine the following local repository structure

```text
src/
  backend/
    features/
      backend_feature_a.feature
      backend_feature_a.feature
      specsync.json
  frontend/
    features/
      frontend_feature_c.feature
      frontend_feature_d.feature
      specsync.json
  specsync.json
```

In this setup, there is a SpecSync configuration file in the `src` folder, that contains all common settings (e.g. repository URL), but this is not used directly for synchronization. For example:

```javascript
{
  "remote": { // common target URL
    "projectUrl": "https://dev.azure.com/myorganization/MyProject",
  },
  "synchronization": { // some common synchronization settings
    "automation": {
      "enabled": true
    }
  }
}
```

The `specsync.json` files in `src/backend/features` and `src/frontend/features` contain all the frontend and backend specific details (e.g. the Test Suite to be synchronize the scenarios to). For example the one in the backend would look like:

```javascript
{
  "remote": { 
    "testSuite": {
      "name": "Backend BDD Scenarios"
    }
  }
}
```

If the synchronization is performed from the specific features folder, SpecSync will automatically use the specific configuration file and also automatically load the `specsync.json` file as well from the `src` folder, because all parent folders are searched for configuration files.

```text
...src/backend/features> dotnet specsync push
```

{% hint style="info" %}
In order to avoid accidental synchronizations performed from the root folder using the parent configuration file, you can consider renaming it to a different name (e.g. `specsync-parent.json`). In this case you need to specify the path of the parent configuration in the specific configuration files using the `toolSettings/parentConfig` setting.
{% endhint %}

You can also invoke the synchronization from another folder, but in this case you need to specify the path to the configuration file.

```text
...src> dotnet specsync push backend/features/specsync.json
```

### Configuring multiple feature-sets by placing multiple configuration files to the root folder

For this option, let's imagine a folder structure, like this:

```text
src/
  backend/
    features/
      backend_feature_a.feature
      backend_feature_a.feature
  frontend/
    features/
      frontend_feature_c.feature
      frontend_feature_d.feature
  specsync-parent.json
  specsync-backend.json
  specsync-frontend.json
```

With this approach all configuration files are stored in a root folder (in the `src` folder in our case). 

The `specsync-parent.json` contains the common settings (e.g. repository URL). You can find an example for the file content in the previous section where we discussed the *configuration per folder* option.

The `specsync-backend.json` and the `specsync-frontend.json` files contain the settings specific to the different feature-sets, but since they are placed in the root folder now, they need to have the necessary settings to configure the local scope and also the parent configuration file. The `specsync-backend.json` configuration file can look like this:

```javascript
{
  "toolSettings": {
    "parentConfig": "specsync-parent.json"
  },
  "local": {
    "featureFileSource": {
      "type": "folder",
      "folder": "backend/features"
    }
  },
  "remote": { 
    "testSuite": {
      "name": "Backend BDD Scenarios"
    }
  }
}
```

The synchronization for the frontend or the backend can be invoked by specifying the configuration file:

```text
...src> dotnet specsync push specsync-backend.json
```

### Synchronize multiple feature-sets from the same folder <a href="multiple-feature-set-same-folder" id="multiple-feature-set-same-folder"></a>

The *different configuration files in root* option that was shown in the previous section can also be used if the feature files are all in the same folder, and the scenarios are only separated by tags or file name conventions.

Let's imagine the following folder structure:

```text
src/
  features/
    common_features_a.feature
    backend_feature_b.feature
    frontend_feature_c.feature
  specsync-parent.json
  specsync-backend.json
  specsync-frontend.json
```

The scenarios in the `backend_feature_b.feature` are all tagged with `@backend`, the scenarios in `frontend_feature_c.feature` with `@frontend` and the ones in `common_features_a.feature` are tagged with both `@frontend` and `@backend`. 

The `specsync-parent.json` contains the common settings and looks similar to the ones in the previous sections, but here we can even specify the main target folder:

```javascript
{
  "local": {
    "featureFileSource": {
      "type": "folder",
      "folder": "features" // the common target folder
    }
  },
  "remote": { // common target URL
    "projectUrl": "https://dev.azure.com/myorganization/MyProject",
  },
  "synchronization": { // some common synchronization settings
    "automation": {
      "enabled": true
    }
  }
}
```

The `specsync-backend.json` and the `specsync-frontend.json` files contain the settings specific to the different feature-sets by specifying the local scope using tags. The `specsync-backend.json` configuration file can look like this:

```javascript
{
  "toolSettings": {
    "parentConfig": "specsync-parent.json"
  },
  "local": {
    "tags": "@backend"
  },
  "remote": { 
    "testSuite": {
      "name": "Backend BDD Scenarios"
    }
  }
}
```

{% hint style="info" %}
Instead of the `local/tags` you could also separate the scenarios by file name patterns using the `local/sourceFiles` setting. You can find more information about this in the [Excluding scenarios from synchronization](../features/common-synchronization-features/excluding-scenarios-from-synchronization.md) page.
{% endhint %}

The synchronization for the frontend or the backend can be invoked by specifying the configuration file:

```text
...src> dotnet specsync push specsync-backend.json
```
