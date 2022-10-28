# Filters and scopes

SpecSync synchronizes scenarios from feature files to Azure DevOps test cases when using the push command. For that you have to specify which feature files and which scenarios within the feature files should be considered by SpecSync. This is the *synchronization local scope* or **local scope** in short.

Similarly to this, when using the pull command \(for [two-way synchronization](../features/pull-features/two-way-synchronization.md)\), you have to specify the the scope of the test cases that should be considered. This is the *synchronization local scope* or **remote scope**.

Although the *local scope* is primarily used for the the push and the *remote scope* primarily for the pull command, they are both used for both operation actually: the pull command uses the *local scope* to find changes in the linked Test Cases; the push command can detect removed scenarios using the *remote scope*.

In general, the **synchronization scope** defines the set of artifacts \(scenarios or Test Cases\) that are to be synchronized. In some cases you don't want to synchronize the entire scope though, but only a subset of them. For example while working on a particular story, you only want to synchronize the scenarios related to that story. This can be specified by the **synchronization filter**.

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

Specifying the remote scope (i.e. the Test Cases that are considered to be synchronized from scenarios) is useful, because if it is specified, SpecSync can recognize that a scenario has been removed from the feature file. In that case the Test Case related to the missing scenario is not deleted but tagged with `specsync:removed`. You can review the Test Cases of this tag and delete them manually. (The tag is removed from the Test Case if the scenario is restored.)

Currently the remote scope can be specified by configuring a static Test Suite where the synchronized Test Cases will be included automatically by SpecSync. See [Include synchronized Test Cases to a Test Suite](../features/common-synchronization-features/group-synchronized-test-cases-to-a-test-suite.md) for details.

For the pull command the remote scope is mandatory. Read more about this in [Two-way synchronization](../features/pull-features/two-way-synchronization.md).

## Setting local filter

To synchronize only a subset of the scenarios within the local scope you can specify a tag (`--tagFilter`) or a feature file filter (`--sourceFileFilter`) expression as a command line option. \(See [Command line reference](../reference/command-line-reference/) for details.\)

For example to only synchronize the scenarios related to a particular user story \(tagged with `@story:123`\), the following option can be used.

```text
dotnet specsync push --tagFilter "@story:123"
```

To only synchronize a particular feature file in a sub-folder (e.g. `order.feature`), the following option can be used.

```text
dotnet specsync push --sourceFileFilter "**/order.feature"
```
