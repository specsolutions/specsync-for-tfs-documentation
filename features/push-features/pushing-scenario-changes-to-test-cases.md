# Pushing scenario changes to Test Cases

SpecSync can synchronize scenarios and scenario execution results with Azure DevOps Test Cases. The most commonly used command to achieve this is to upload \(push\) the scenario changes to Test Cases that is provided using the SpecSync [`push` command](../../reference/command-line-reference/push-command.md).

The SpecSync push command processed all scenarios in the [local feature set](../../important-concepts/how-to-define-the-local-feature-set-to-be-synchronized.md) \(e.g. the `features` folder of your project\), detects if there were any change since the last synchronization and updates the Test Case if necessary.

You can find all available command line options for the push command in the [push command line reference](../../reference/command-line-reference/push-command.md). The configuration options related to push are in the [synchronization](../../reference/configuration/configuration-synchronization/) section of the configuration file.

Depending on the changes of the scenario, the push command performs different actions in Azure DevOps as the following table shows.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Changes</th>
      <th style="text-align:left">Actions</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">The scenario is new (not yet linked to a Test Case)</td>
      <td style="text-align:left">A new Test Case work item is created and the scenario is tagged with a
        link tag (e.g. <code>@tc:1234</code>) to document the ID of the created
        Test Case.</td>
    </tr>
    <tr>
      <td style="text-align:left">The scenario is linked and up-to-date with the Test Case</td>
      <td style="text-align:left">No action is taken</td>
    </tr>
    <tr>
      <td style="text-align:left">The scenario has been changed since the last synchronization</td>
      <td style="text-align:left">The Test Case is updated with the changes. (No change in the feature file.)</td>
    </tr>
    <tr>
      <td style="text-align:left">The scenario has been synchronized, but since the last synchronization
        the Test Case has been updated.</td>
      <td style="text-align:left">
        <p>The push command overwrites the changes of the Test Case fields that are
          maintained by SpecSync (e.g. title, steps, tags). The changes in other
          fields (e.g. description) are not changed.</p>
        <p>In case you would like to preserve the changes of the fields maintained
          by SpecSync, you have to use the <a href="../pull-features/two-way-synchronization.md">pull command</a> first.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">The scenario has been removed</td>
      <td style="text-align:left">SpecSync does not delete the related Test Case. If SpecSync is configured
        to include the Test Cases to a Test Suite (recommended), the Test Case
        will be tagged with <code>specsync:removed</code>. You can review the Test
        Cases of this tag and delete them manually. (The tag is removed from the
        Test Case if the scenario is restored.)</td>
    </tr>
  </tbody>
</table>

{% hint style="info" %}
The SpecSync push command needs to modify the feature file when a new scenario is synchronized. The feature file change has to be preserved in the source control \(commit & push\), otherwise when other users perform a push, Test Case duplicates will be created.

When performing the push on a build pipeline, or in any other cases where local changes cannot be preserved, the push command has to be invoked with the `--disableLocalChanges` switch. More information about using SpecSync in build pipelines can be found in the [Use SpecSync from build or release pipeline](../../important-concepts/synchronizing-test-cases-from-build.md) guide.
{% endhint %}

## Test Case fields updated by the push command

The push command updates certain fields of the linked Azure DevOps Test Case to represent the scenario changes as the following table shows.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Updated Test Case field</th>
      <th style="text-align:left">Updated with</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Title</td>
      <td style="text-align:left">Updated based on the scenario name. By default it is set to <code>Scenario: &lt;scenario-name&gt;</code> or <code>Scenario Outline: &lt;scenario-outline-name&gt;</code>.
        The <em>Scenario</em> and <em>Scenario Outline</em> prefixes can be skipped,
        see <a href="configuring-the-format-of-the-synchronized-test-cases.md">Configuring the format of the synchronized test cases</a>.</td>
    </tr>
    <tr>
      <td style="text-align:left">Steps</td>
      <td style="text-align:left">
        <p>Updated based on the scenario steps. By default each scenario step is
          represented by a Test Case step, but the synchronization can be configured
          to use the <em>Expected Result</em> field of the Test Case step for the <em>Then</em> steps.
          About this and other step-related formatting option can be configured,
          see <a href="configuring-the-format-of-the-synchronized-test-cases.md">Configuring the format of the synchronized test cases</a> for
          details.</p>
        <p>By default all Test Case steps are overwritten by the scenario steps,
          but you can configure SpecSync to keep certain Test Case steps based on
          a prefix. See details in <a href="customization-ignoring-marked-test-case-steps.md">Customization: Ignoring marked Test Case steps</a>.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Tags</td>
      <td style="text-align:left">
        <p>Updated based on the scenario tags. (The tags placed on the feature block
          are also replicated in the Test Case.)</p>
        <p>The tag synchronization can be customized with <a href="customization-mapping-tags.md">Customization: Mapping tags</a> and
          <a
          href="customization-ignoring-test-case-tags.md">Customization: Ignoring Test Case Tags</a>.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Parameter values</td>
      <td style="text-align:left">The scenario outlines are synchronized to parametrized Test Cases. The
        values provided in the scenario outline <em>Examples </em>block are saved
        as parameter values. Multiple examples blocks are merged into one Test
        Case parameter value table. See <a href="synchronizing-scenario-outlines.md">Synchronizing Scenario Outlines</a> for
        details.</td>
    </tr>
    <tr>
      <td style="text-align:left">Links</td>
      <td style="text-align:left">The manually edited Test Case links are never removed by SpecSync, but
        you can configure SpecSync to detect scenario tags that refer to an Azure
        DevOps work item (e.g. <code>@bug:1234</code>) and create Test Case links
        from them. See <a href="../common-synchronization-features/linking-work-items-with-tags.md">Linking Work Items using tags</a> for
        details.</td>
    </tr>
    <tr>
      <td style="text-align:left">State</td>
      <td style="text-align:left">By default the push command does not change the value of the State field,
        but you can configure it to reset the state to a specific value (e.g. <code>Design</code>)
        whenever the Test Case is changed. See <a href="setting-test-case-state-on-change.md">Setting Test Case State on change</a> for
        details.</td>
    </tr>
    <tr>
      <td style="text-align:left">Area and Iteration</td>
      <td style="text-align:left">By default the push command does not change the value of the Area and
        the Iterations fields, but you can configure it initialize these fields
        with a configured value (e.g. <code>MyProject\Team1</code>) when the Test
        Case is initially created. See <a href="add-new-test-cases-to-an-area-or-an-iteration.md">Add new Test Cases to an Area or an Iteration</a> for
        details.</td>
    </tr>
    <tr>
      <td style="text-align:left">Automation Status</td>
      <td style="text-align:left">By default the push command creates the Test Cases as <code>Not Automated</code>.
        To be able to synchronize automated Test Cases (for all or for selected
        scenarios), you can configure SpecSync for this. See <a href="mark-test-cases-as-automated.md">Mark Test Cases as Automated</a> for
        details.</td>
    </tr>
    <tr>
      <td style="text-align:left">Associated Automation</td>
      <td style="text-align:left">For MsTest-based SpecFlow scenarios, SpecSync can also configure the <em>Associated Automation</em> field
        with the test method generated by SpecFlow for the scenario. This can be
        used for <a href="../test-result-publishing-features/support-for-azure-devops-test-plan-test-suite-based-test-execution.md">Test-suite based execution</a>.
        In order to synchronize scenario execution results, you can also consider
        the SpecSync <a href="../test-result-publishing-features/">Test result publishing features</a> that
        provide a more flexible solution.</td>
    </tr>
  </tbody>
</table>

## Include synchronized Test Cases to a Test Suite

Azure DevOps provides different features \(e.g. to be able to record test results\) for Test Cases that are included in Test Suites. The Test Cases synchronized by SpecSync can be included in multiple Test Suites manually, but SpecSync can be configured to add the Test Cases to a selected Static Test Suite.

Adding the Test Cases to a Test Suite is recommended, because with that SpecSync can detect is a scenario has been removed. See more information about how to configure the Test Suite for the Test Cases in the [Include synchronized Test Cases to a Test Suite](../common-synchronization-features/group-synchronized-test-cases-to-a-test-suite.md) page.

