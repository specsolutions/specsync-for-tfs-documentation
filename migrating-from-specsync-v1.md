# Migrating from SpecSync v1

SpecSync v2 has introduced a couple of major improvements. If you have used SpecSync v1.* earlier, you need to do a few steps in order to use v2 in your project.

You can find a complete list of improvements in the [Changelog](changelog.md).

## Replace NuGet package references

If you have used SpecSync via NuGet, you have to replace the NuGet package references. 

The core package (`SpecSync.MTM`) has been renamed to `SpecSync.TFS` and it no longer contains the SpecFlow plugin needed for synchronizing automated test cases. 

If you use SpecSync with SpecFlow and have used synchronizing automated test cases before, you have to add an additional NuGet package. The necessary NuGet package depends on the SpecFlow version you use. For example for SpecFlow v2.3.*, you have to use the [`SpecSync.TFS.SpecFlow.2-3`](https://www.nuget.org/packages/SpecSync.TFS.SpecFlow.2-3) package. Read more about this in [Synchronizing automated test cases](synchronizing-automated-test-cases.md).

## Capture your synchronization settings in a config file

With SpecSync v1 all the configuration had to be specified as command line options but as the number of different configuration options grew, this became inconvenient. In v2 we have introduced a JSON configuration file to keep the configuration settings clean and organized. The configuration file (called `specsync.json` by default) has a JSON schema, therefore most of the IDE editors (e.g. Visual Studio or Visual Studio Code) offers auto-completion and validation for it, so that changing the configuration settings becomes easy. The JSON schema provides a short summary of each element that you can see if you hover your mouse over the setting, but there is also a complete reference of the different setting with examples in the [Configuration](configuration.md) reference.

Some of the settings can still be overridden from command line (e.g. password or force update). You can find a complete list of these in the [Usage](usage.md) reference.

When upgrading from v1, you have to create a `specsync.json` file (based on an [empty](http://schemas.specsolutions.eu/specsync-empty.json) or a [detailed](http://schemas.specsolutions.eu/specsync-sample.json) sample), and migrate your synchronization settings to it. Most of the settings have the same name, but we have renamed some of the settings for better clarity. If you get into trouble with finding the right setting in the new configuration file format, please contact us at specsync@specsolutions.eu.

## Review revised two-way synchronization workflow

If you have used two-way synchronization with v1 (also called as "back-sync"), you should check the new workflow in the [Two-way synchronization](two-way-synchronization.md) guide. In the new workflow, we have split the two synchronization ways into separate commands: push and pull.  

## Use new name and push/pull command when invoking the synchronization tool

Since the settings have moved from command line options to the configuration file, calling the synchronization tool from command line becomes easier. 

Be aware that the command line tool has been renamed to `SpecSycn4TFS.exe`.

In addition to renaming, the tool supports multiple synchronization commands. For synchronizing scenarios to test cases, the `push` command has to be used. See full list of commands and options in the [Usage](usage.md) reference.

The usual way of invoking the synchronization is now as simple as this:

```
path-to-specsync-package\tools\SpecSync4TFS.exe push
```
