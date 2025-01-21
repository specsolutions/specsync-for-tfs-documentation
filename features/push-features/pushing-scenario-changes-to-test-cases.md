# Pushing scenario changes to Test Cases

SpecSync can synchronize scenarios and scenario execution results with Azure DevOps Test Cases. The most commonly used command to achieve this is to upload (push) the scenario changes to Test Cases that is provided using the SpecSync [`push` command](../../reference/command-line-reference/push-command.md).

The SpecSync push command processed all scenarios in the [local feature set](../../important-concepts/how-to-define-the-local-feature-set-to-be-synchronized.md) (e.g. the `features` folder of your project), detects if there were any change since the last synchronization and updates the Test Case if necessary.

You can find all available command line options for the push command in the [push command line reference](../../reference/command-line-reference/push-command.md). The configuration options related to push are in the [synchronization](../../reference/configuration/configuration-synchronization/) section of the configuration file.

The connection between the scenario and the Test Case in Azure DevOps is established with a *test case link tag*. By default the link tag uses the format `@tc:1234`, but both the `tc` prefix and the separator `:` can be configured (with `synchronization/tagPrefixSeparators` and `synchronization/linkLabelSeparator`), so you can also have test case links that use different format, e.g. `@TestCase=1234`. In the documentation we use the default test case link tag format.

Depending on the changes of the scenario, the push command performs different actions in Azure DevOps as the following table shows.

| Changes | Actions |
| ------- | ------- |
| The scenario is new (not yet linked to a Test Case) | A new Test Case work item is created and the scenario is tagged with a link tag (e.g. `@tc:1234`) to document the ID of the created Test Case. |
| The scenario is linked and up-to-date with the Test Case | No action is taken |
| The scenario has been changed since the last synchronization | The Test Case is updated with the changes. (No change in the feature file.) |
| The scenario has been synchronized, but since the last synchronization the Test Case has been updated. | <p>The push command by default overwrites the changes of the Test Case fields that are maintained by SpecSync (e.g. title, steps, tags). The changes in other fields (e.g. description) are not changed.</p><p>A different conflict handling method can also be configured. See [Synchronization conflict resolution](../common-synchronization-features/synchronization-conflict-resolution.md) for details.</p><p>In case you would like to preserve the changes of the fields maintained by SpecSync, you have to use the <a href="../pull-features/two-way-synchronization.md">pull command</a> first.</p> |
| The scenario has been removed | SpecSync does not delete the related Test Case. If SpecSync is configured to include the Test Cases to a Test Suite (recommended), the Test Case will be tagged with `specsync:removed`. You can review the Test Cases of this tag and delete them manually. (The tag is removed from the Test Case if the scenario is restored.) |

{% hint style="info" %}
The SpecSync push command needs to modify the feature file when a new scenario is synchronized. The feature file change has to be preserved in the source control (commit & push), otherwise when other users perform a push, Test Case duplicates will be created.

When performing the push on a build pipeline, or in any other cases where local changes cannot be preserved, the push command has to be invoked with the `--disableLocalChanges` switch. More information about using SpecSync in build pipelines can be found in the [Use SpecSync from build or release pipeline](../../important-concepts/synchronizing-test-cases-from-build.md) guide.
{% endhint %}

{% hint style="info" %}
You can perform the push operation with the `--linkOnly` flag so that it only links new scenarios and does not update existing Test Cases.
{% endhint %}

{% hint style="info" %}
SpecSync operations, including the push command supports "dry-run" mode using the `--dryRun` command line option. In dry-run mode, no change is made neither to Azure DevOps nor to the feature files, so you can test the impact of an operation without making an actual change.
{% endhint %}

## Test Case fields updated by the push command

The push command updates certain fields of the linked Azure DevOps Test Case to represent the scenario changes as the following table shows.

| Updated Test Case field | Updated with |
| ----------------------- | ------------ |
| Title | Updated based on the scenario name. By default it is set to `Scenario: <scenario-name>` or `Scenario Outline: <scenario-outline-name>`. The _Scenario_ and _Scenario Outline_ prefixes can be skipped, see [Configuring the format of the synchronized test cases](configuring-the-format-of-the-synchronized-test-cases.md). For special situations the synchronization of the Test Case title can be omitted, see [Customization: Do not synchronize title](customization-do-not-synchronize-title.md). |
| Steps | <p>Updated based on the scenario steps. By default each scenario step is represented by a Test Case step, but the synchronization can be configured to use the <em>Expected Result</em> field of the Test Case step for the <em>Then</em> steps. About this and other step-related formatting option can be configured, see <a href="configuring-the-format-of-the-synchronized-test-cases.md">Configuring the format of the synchronized test cases</a> for details.</p><p>By default all Test Case steps are overwritten by the scenario steps, but you can configure SpecSync to keep certain Test Case steps based on a prefix. See details in <a href="customization-ignoring-marked-test-case-steps.md">Customization: Ignoring marked Test Case steps</a>.</p> |
| Tags | <p>Updated based on the scenario tags. (The tags placed on the feature block are also replicated in the Test Case.)</p><p>The tag synchronization can be customized with <a href="customization-mapping-tags.md">Customization: Mapping tags</a> and <a href="customization-ignoring-test-case-tags.md">Customization: Ignoring Test Case Tags</a>.</p> |
| Parameter values | The scenario outlines are synchronized to parametrized Test Cases. The values provided in the scenario outline _Examples_ block are saved as parameter values. Multiple examples blocks are merged into one Test Case parameter value table. See [Synchronizing Scenario Outlines](synchronizing-scenario-outlines.md) for details. |
| Links | The manually edited Test Case links are never removed by SpecSync, but you can configure SpecSync to detect scenario tags that refer to an Azure DevOps work item (e.g. `@bug:1234`) and create Test Case links from them. See [Linking Work Items using tags](../common-synchronization-features/linking-work-items-with-tags.md) for details. |
| State | By default the push command does not change the value of the State field, but you can configure it to reset the state to a specific value (e.g. `Design`) whenever the Test Case is changed. See [Setting Test Case State on change](setting-test-case-state-on-change.md) for details. |
| Area and Iteration | By default the push command does not change the value of the Area and the Iterations fields, but you can configure it initialize these fields with a configured value (e.g. `MyProject\Team1`) when the Test Case is initially created. See [Add new Test Cases to an Area or an Iteration](add-new-test-cases-to-an-area-or-an-iteration.md) for details. |
| Automation Status | By default the push command creates the Test Cases as `Not Automated`. To be able to synchronize automated Test Cases (for all or for selected scenarios), you can configure SpecSync for this. See [Mark Test Cases as Automated](mark-test-cases-as-automated.md) for details. |
| Associated Automation | For MsTest-based SpecFlow or Reqnroll scenarios, SpecSync can also configure the _Associated Automation_ field with the test method generated by SpecFlow or Reqnroll for the scenario. This can be used for [Test-suite based execution](../test-result-publishing-features/support-for-azure-devops-test-plan-test-suite-based-test-execution.md). In order to publish scenario execution results, you can also consider the SpecSync [Test result publishing features](../test-result-publishing-features/) that provide a more flexible solution. |
| Attachments | The manually uploaded Test Case attachments are never removed by SpecSync, but you can configure SpecSync to detect scenario tags that refer to attached file and upload Test Case attachments from them. See [Attach files to Test Cases using tags](attach-files.md) for details. |

## Include synchronized Test Cases to a Test Suite

Azure DevOps provides different features (e.g. to be able to record test results) for Test Cases that are included in Test Suites. 

The Test Cases can be automatically sycnhronized with a single static Test Suite by defining it as "remote scope". This is recommended, because with that SpecSync can detect is a scenario has been removed. See [Include synchronized Test Cases to a Test Suite](../common-synchronization-features/group-synchronized-test-cases-to-a-test-suite.md) for details.

Alternatively or in addition to that, you can configure additional Test Suites where the Test Cases should be included based on various conditions. See [Customization: Add Test Cases to Suites](customization-add-test-cases-to-suites.md) for details.

The synchronized Test Cases can also be added to various Test Suites manually or by defining query-based or requirement-based suites in Azure DevOps.
