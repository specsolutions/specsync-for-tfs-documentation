# Two-way synchronization

In the majority of the cases, the scenarios are changed in the feature files. This ensures that all necessary automation infrastructure \(eg. step definitions\) are also adapted when a change has been performed in the scenario.

In some special cases, it might be useful to be able to make small changes in the test cases synchronized from the scenarios directly in Azure DevOps. Synchronizing such changes back to the feature files is a complex process and it is only recommended if it's really required.

SpecSync provides a feature for two-way synchronization using the [`pull` command](../reference/command-line-reference.md).

In order to use the two-way synchronization feature efficiently, we recommend having a team agreement about whether the feature file or the Azure DevOps test case is the primary source of information at a time. For example, in the sprint planning preparation phase, the changes are made in the Azure DevOps test case, but once the sprint has been started, the changes are made in the feature file first.

_Note: The two-way synchronization is an_ [_Enterprise feature_](../licensing.md)_._

## Two-way synchronization workflow

In order to be able to use two-way synchronization, it has to be enabled in the SpecSync [configuration file](../configuration/) in the [`synchronization/pull` section](../configuration/configuration-synchronization/configuration-synchronization-pull.md):

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
2. Pull test case changes from Azure DevOps:

   ```text
   path-to-specsync-package\tools\SpecSync4AzureDevOps.exe pull
   ```

3. During the pull operation SpecSync might ask you to help resolving conflicting changes.
4. Verify if the changes are correct \(compiles, correct Gherkin syntax, steps are defined, etc.\)
5. Push back to Azure DevOps \(we recommend this step even if there were no fixes required, because this way SpecSync can register the current state of the scenarios in Azure DevOps\)

   ```text
   path-to-specsync-package\tools\SpecSync4AzureDevOps.exe push
   ```

6. Check in \(commit and push\) your changes to source control

_Note: SpecSync cannot ensure that the updated feature files are committed to the source control. If these changes are not committed, and someone else is also performing the synchronization, the changes will be synchronized multiple times causing various conflicts. We recommend to ensure that there are no parallel pull operations are performed._

## Creating new scenarios from test cases

By default, the pull command only loads the changes of the test cases that have been linked to a scenario already \(i.e. have been synchronized with SpecSync push once\).

You can enable creating new scenarios during pull using the [`synchronization/pull/enableCreatingScenariosForNewTestCases` configuration setting](../configuration/configuration-synchronization/configuration-synchronization-pull.md):

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

This setting works together with the test suite synchronization and will only create scenarios from the specified test suite. See more about using test suites in [Group synchronized test cases to a test suite](group-synchronized-test-cases-to-a-test-suite.md).

As a result of the pull operation with this setting, SpecSync will create a new feature file for each new test case in the test suite. The feature files will be named based on the test case ID, e.g. `12345.feaure`. It is recommended to review and group these scenarios to other feature files. When moving the scenarios, make sure you also move the test case link tag \(e.g. `@tc:12345`\) together with the scenario to keep the link to the test case.

