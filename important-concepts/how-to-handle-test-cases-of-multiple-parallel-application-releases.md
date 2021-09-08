# How to handle Test Cases of multiple parallel application releases

For some applications or products the development team need to maintain multiple main product versions. For example, you need to keep maintaining v1 of the product \(provide bugfixes, etc.\) although you have already released v2 as well.

Handling such multiple versions in source code is easy with branching, but if your source code is connected to different work items in Azure DevOps \(including Test Cases\), you have to consider your release strategy, because Azure DevOps work items do not support having multiple parallel active versions or linking to a specific history item of a work item.

For the requirement work items, teams usually make categories of release-dependent and release-agnostic work items. For example, the User Story work item always belongs to a release, but the feature work item is independent of the release and you have to refer the same feature from multiple releases. 

This problem is especially interesting for Test Cases, because although the same Test Cases usually have to be verified for multiple releases, they might also need to be changed to match the new capabilities of the new release. In more detail, in case of a new v2 release, the following cases are possible:

* _Regression:_ Test Case of v1 is valid and needs to be verified for v2
* _Revised regression:_ Test Case of v1 is valid, but needs a slight modification to be verified in v2 \(e.g. add a new preparation  step\)
* _New functionality:_ A new Test Case is added for v2 that did not exist in v1
* _Removed functionality:_ Test Case of v1 is not valid anymore for v2

In Azure DevOps, the test cases can be grouped to Test Plans and be organized within a Test Plan into Test Suites. Both Test Plans and Test Suites contain only a _reference_ to the Test Cases, so if a Test Case changes, the changes became automatically visible in all Test Plans and Test Suites where they are included.

A general recommendation of the [Azure DevOps documentation](https://docs.microsoft.com/en-us/azure/devops/test/create-a-test-plan?view=azure-devops) to maintain a separate Test Plan for the Test Cases in case you have to maintain multiple releases. In case of a new v2 release, this can be established by 

* creating a new Test Plan for v2 that refers to \(contains\) the _same Test Cases_ as the v1 Test Plan, or
* creating a new Test Plan for v2 that contains a _cloned version of the existing Test Cases_ of v1

Both options have advantages and disadvantages. In the following sections we detail these strategies and describe how they can be used with Test Cases synchronized by SpecSync.

{% hint style="info" %}
Regardless of the approach you choose, be careful with requirement-based Test Suites that are made for release-agnostic work items \(e.g. a feature\) or with query-based Test Suites that do not include a clause that filters for the release the Test Case is related to \(e.g. iteration path\). These suites might be able to include Test Cases into the Test Plans implicitly that are not valid for that release.
{% endhint %}

## Creating a new Test Plan with the existing Test Cases

This approach is more practical when the old versions have to be supported for a short period of time only \(at least with Test Case level traceability\) or when the existing tests are changing over releases rarely.

To be able to achieve this model, when a new release is started, you need to

* Create a new Test Plan or use the "Copy test plan" function of Azure DevOps using the "Reference existing test cases" setting.
* Make sure that all existing Test Cases are included, except the ones related to removed functionality and remove the related scenarios as well from the feature files.
* Change the `specsync.json` configuration file on the branch of the new release and specify a Test Suite in the new Test Plan
* Synchronize the scenarios with SpecSync from the different release branches -- they will add/remove the test cases from the Test Suite in the Test Plan of the release.

Revised regression: If a scenario needs to be changed in the new release then you might run into a "ping-pong" effect, since the synchronization from the different release branches would always change the related Test Case to the version on that release. To avoid that, you have the following options

1. You don't synchronize the scenarios \(or at least the changed scenarios -- marked with a tag\) in the old release. This might sound bad, but if the old release is mainly kept for hotfixes only, the scenarios are usually anyway not affected. Of course the Test Cases will be updated by the new release and you will see these updated version even if you browse the Test Case from the Test Plan of the old release, but this might be an acceptable inconsistency. 
2. You duplicate the Test Cases of the changed scenarios. This can be done by simply remove the @tc tags \(e.g. @tc:1234\) from these scenarios and running the SpecSync push command. This command will create a new Test Case for the scenario, remove the old Test Case from the configured Test Suite and add the new to it. Since the old Test Case remain unchanged, the old release will stay as it is. 
3. Let SpecSync duplicate the Test Cases of changed scenarios and keep the reference to the old Test Case as well in the feature file. For this, you have to enable the [branch-tag feature](../features/push-features/support-synchronizing-scenarios-from-a-branch.md), where you can define a tag prefix for the new release \(called _branch tag_, e.g. `tc-rel-2`\) that takes precedence over the default prefix. When a scenario is detected to be changed and there is no branch tag on the scenario yet, SpecSync will create a new Test Case with the changed details and add a branch tag to the scenario, but keeps the old tag as well. If the scenario happens to change again, the Test Case referred by the branch tag is updated. \(Since branch tags can handle one base and one branch tag, when starting a new release, the branch tags has to be replaced by the core tag. I.e. when Release 3 is started, all `@tc-rel-2:1234` like tags should be replaced by `@tc:1234` and the old `@tc` tag has to be removed.\)

## Creating a new Test Plan with clones of existing Test Cases

This approach is more practical if you need to make parallel maintained releases rarely, but those releases have more active development.

To be able to achieve this model, when a new release is started, you need to

* Create a new Test Plan with an empty Test Suite for the Test Cases synchronized with SpecSync \(don't use "Copy test plan", see note below\)
* Change the `specsync.json` configuration file on the branch of the new release and specify a Test Suite in the new Test Plan 
* Delete all @tc:XXX tags from the scenarios on the branch of the new release. This can be done in Visual Studio by performing a "Replace in Files" command with "Use regular expressions" enabled, `@tc:\d+` as search text and an empty replacement text. \(Alternatively you can keep the tags on the scenarios, but specify a new Test Case tag prefix \(e.g. `tc-rel-2`\) in the [`synchronization/testCaseTagPrefix`](../reference/configuration/configuration-synchronization/) setting of the configuration file. If you also enable the old prefix as [work item link](../features/common-synchronization-features/linking-work-items-with-tags.md), the newly created Test Cases will even have a reference to the version in the previous release even.\)
* Invoke the SpecSync push command from the branch of the new release. This will create new Test Cases of the scenarios and add tags to the scenarios for them. These Test Cases will be essentially the clones of the Test Cases in the previous release, as they both have been synchronized from the scenarios that are not changed yet.
* Synchronize the scenarios with SpecSync from the different release branches -- they will add/remove the test cases from the Test Suite in the Test Plan of the release and change the Test Cases related to the particular release.

{% hint style="info" %}
It is currently not recommended to use the "Copy test plan" feature of Azure DevOps with the "Duplicate existing test cases" option, as you would need to manually update the Test Case tags in the scenarios to update the cloned Test Cases.

A feature is currently in preparation, where you could select a recent "Copy test plan" operation and based on its results SpecSync would update all Test Case links. This is planned to be implemented in Q3 2021.

If your Test Plans contain many Test Cases that are not synchronized by SpecSync \(e.g. manual test\), you can consider using the "Copy test plan" with the duplicate option, but delete the cloned Test Cases that are related to the scenarios. SpecSync will anyway create the clones of those.
{% endhint %}





