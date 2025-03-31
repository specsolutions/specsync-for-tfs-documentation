# Pulling Test Case changes to local scenarios

In the majority of the cases, the scenarios are changed in the feature files. This ensures that all necessary automation infrastructure \(eg. step definitions\) are also adapted when a change has been performed in the scenario.

In some special cases, it might be useful to be able to make small changes in the Test Cases synchronized from the scenarios directly in Azure DevOps. Synchronizing such changes back to the feature files is a complex process and it is only recommended if it's really required.

SpecSync provides a feature for two-way synchronization using the [`pull` command](../../reference/command-line-reference/pull-command.md).

In order to use the two-way synchronization feature efficiently, we recommend having a team agreement about whether the feature file or the Azure DevOps Test Case is the primary source of information at a time. For example, in the sprint planning preparation phase, the changes are made in the Azure DevOps Test Case, but once the sprint has been started, the changes are made in the feature file first.

{% hint style="info" %}
The pull command (two-way synchronization) is an [*Enterprise feature*](../../licensing.md).
{% endhint %}


## Two-way synchronization workflow

In order to be able to use two-way synchronization, it has to be enabled in the SpecSync [configuration file](../../reference/configuration/) in the [`synchronization/pull` section](../../reference/configuration/configuration-synchronization/configuration-synchronization-pull.md):

```text
{
  ...
  "synchronization": {
    ...
    "pull": {
      "enabled": true
    },
    ...
  },
  ...
}
```

Once this configuration has been done, the general workflow could be:

1. Make sure the project compiles, tests pass and the modified files are checked in to source control.
2. Pull Test Case changes from Azure DevOps:

   ```text
   dotnet specsync pull
   ```

3. During the pull operation SpecSync might ask you to help resolving conflicting changes.
4. Verify if the changes are correct \(compiles, correct Gherkin syntax, steps are defined, etc.\)
5. Push back to Azure DevOps (we recommend this step even if there were no fixes required, because this way SpecSync can register the current state of the scenarios in Azure DevOps)

   ```text
   dotnet specsync push
   ```

6. Check in \(commit and push\) your changes to source control

{% hint style="info" %}
SpecSync cannot ensure that the updated feature files are committed to the source control. If these changes are not committed, and someone else is also performing the synchronization, the changes will be synchronized multiple times causing various conflicts. We recommend to ensure that there are no parallel pull operations are performed.
{% endhint %}

{% hint style="info" %}
In certain cases SpecSync is not able to restore feature-level Gherkin constructs (e.g. feature-level tags or "Background" steps) without changing the behavior of the other scenarios in the same feature file. In this cases the pull command will finish with a warning and you need to review the performed changes manually.
{% endhint %}

{% hint style="info" %}
SpecSync operations, including the pull command supports "dry-run" mode using the `--dryRun` command line option. In dry-run mode, no change is made neither to Azure DevOps nor to the feature files, so you can test the impact of an operation without making an actual change.
{% endhint %}

## Pulling links

If the Test Cases you pull from contain work item links, SpecSync will try to create [work items link tags](../common-synchronization-features/linking-work-items-with-tags.md) from them. 

SpecSync attempts to use the best tag prefix if there are multiple available by selecting the first with matching `targetType` or the first of the ones without `targetType` otherwise. For the best experience for pull, it is recommended to avoid prefixes with the same `targetType` or having multiple link prefix without `targetType`.

If a work item link is removed from the Test Case, the pull operation will remove the corresponding link tag from the local tets case.

{% hint style="info" %}
Pulling links is supported from v3.4.
{% endhint %}

## Creating new scenarios from Test Cases


By default, the pull command only loads the changes of the Test Cases that have been linked to a scenario already (i.e. have been synchronized with SpecSync push once).

You can enable creating new scenarios during pull using the [`synchronization/pull/enableCreatingScenariosForNewTestCases` configuration setting](../../reference/configuration/configuration-synchronization/configuration-synchronization-pull.md):

```text
{
  ...
  "synchronization": {
    ...
    "pull": {
      "enabled": true,
      "enableCreatingScenariosForNewTestCases": true
    },
    ...
  },
  ...
}
```

{% hint style="info" %}
You can perform the pull operation with the `--createOnly` flag so that it only creates new scenarios and does not change the existing ones. With the flag it is not necessary to set the `synchronization/pull/enableCreatingScenariosForNewTestCases` configuration setting.
{% endhint %}

This setting works together with *remote scopes* and will only create scenarios from the Test Cases in the configured remote scope. Read more about configuring remote scopes and the supported remote scope types in [Remote scope](../common-synchronization-features/remote-scope.md) feature documentation.

The process workflow for creating new scenarios for new Test Cases is the following:

1. Create a new Test Case in Azure DevOps
2. Ensure that the new Test Case is included to the remote scope. E.g add the required tag to the Test Case in case of `tag` remote scope type. The exact action depends on the remote scope type, please refer to the [Remote scope](../common-synchronization-features/remote-scope.md) documentation for details.
3. Perform a SpecSync `pull` command with the `--createOnly` flag or set `synchronization/pull/enableCreatingScenariosForNewTestCases` to true and perform a complete `pull` command.

As a result of the pull operation with this setting, SpecSync will create a new feature file for each new Test Case in the remote scope. The feature files will be named based on the Test Case ID, e.g. `12345.feaure`. It is recommended to review and group these scenarios to other feature files. When moving the scenarios, make sure you also move the Test Case link tag (e.g. `@tc:12345`) together with the scenario to keep the link to the Test Case.

## Alternative workflow for creating new scenarios from Test Cases

In the section [above](#creating-new-scenarios-from-test-cases) described a workflow that requires to add the Test Cases to the remote scope manually in Azure DevOps.

In some cases this might not be possible (e.g. the chosen remote scope type does not support manual additions) or moving the newly created local scenario from the temporary location to the final document as a separate step is inconvenient.

In this cases the following alternative workflow can be considered.

1. Create a new Test Case in Azure DevOps
2. Add a *scenario stub* with the ID of the newly created Test Case to the position of any of the existing feature files. See an example scenario stub below.
3. Perform a SpecSync `pull` command.

The *scenario stub* is an empty scenario heading that is tagged with an appropriate Test Case ID tag. Assuming that the ID of the newly created Test Case was `1234`, the scenario stub might look like this (no scenario steps are required):

```gherkin
@tc:1234
Scenario: -
```

During the pull command, SpecSync will detect that the scenario stub is linked to the Test Case and that the scenario details are outdated, so it will replace the scenario title and the scenario steps based on the Test Case:

```gherkin
@tc:1234
Scenario: [title of Test Case]
  [first step of the Test Case, e.g. 'Given some context']
  [second step of Test Case]
  ...
```

In case of mass creation of scenarios using this approach (e.g. from a list of Test Case IDs), you can create a script to generate the scenario stubs. You can review and adapt this [PowerShell script](https://gist.github.com/gasparnagy/b6bec2530497639ea4e3245f0affcdeb), that creates scenario stubs for SpecSync based on a [WIQL](https://learn.microsoft.com/en-us/azure/devops/boards/queries/wiql-syntax?view=azure-devops) query.
