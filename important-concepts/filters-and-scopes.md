# Filters and scopes

SpecSync synchronizes scenarios from feature files to Azure DevOps test cases when using the push command. For that you have to specify which feature files and which scenarios within the feature files should be considered by SpecSync. This is the *synchronization local scope* or **local scope** in short.

Similarly to this, when using the pull command (for [two-way synchronization](../features/pull-features/two-way-synchronization.md)), you have to specify the the scope of the test cases that should be considered. This is the *synchronization local scope* or [**remote scope**](../features/common-synchronization-features/remote-scope.md).

Although the *local scope* is primarily used for the the push and the *remote scope* primarily for the pull command, they are both used for both operation actually: the pull command uses the *local scope* to find changes in the linked Test Cases; the push command can detect removed scenarios using the *remote scope*.

In general, the **synchronization scope** defines the set of artifacts (scenarios or Test Cases) that are to be synchronized. In some cases you don't want to synchronize the entire scope though, but only a subset of them. For example while working on a particular story, you only want to synchronize the scenarios related to that story. This can be specified by the **synchronization filter**.

So scope is a permanent configuration setting, filter is a temporary setting that you want to apply only for a particular synchronization run.

## Setting local scope

The set of feature files and scenarios to be considered for the synchronization can be specified in the [`local` Configuration](../reference/configuration/configuration-local.md).

You can choose about which feature files should be considered

* based on a .NET project
* based on a folder

You can also limit the scope by set of feature files or particular scenario tags. See [Excluding scenarios from synchronization](../features/common-synchronization-features/excluding-scenarios-from-synchronization.md) for details.

The following example sets the local scope to the scenarios that are marked as `@done` but exclude the ones that are tagged with `@ignored` or `@planned`.

```text
{
  ...
  "local": {
    "tags": "@done and not (@ignored or @planned)"
  },
  ...
}
```

## Setting remote scope

Configuring a remote scope is optional, but recommended. If no remote scope is configured SpecSync will not be able to detect removed local test cases and the related option for the pull command will not be available.

For details on how to specify the remote scope and how does SpecSync detects hand processes removed local test cases, please check the [Remote scope](../features/common-synchronization-features/remote-scope.md) documentation.

## Setting local filter

To synchronize only a subset of the scenarios within the local scope you can specify a [local test case condition](../features/general-features/local-test-case-conditions.md) (`--filter`) or a feature file filter (`--sourceFileFilter`) expression as a command line option. (See [Command line reference](../reference/command-line-reference/) for details.)

For example to only synchronize the scenarios related to a particular user story (tagged with `@story:123`), the following option can be used.

```text
dotnet specsync push --filter "@story:123"
```

To only synchronize a particular feature file in a sub-folder (e.g. `order.feature`), the following option can be used.

```text
dotnet specsync push --sourceFileFilter "**/order.feature"
```
