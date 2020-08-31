# Troubleshooting

This page contains common issues and their solution. If you don't find your issues here, please [contact support](specsync-support.md).

### Test result publishing fails with "Mismatch in automation status of test case and test run"

Azure DevOps \(ADO\) collects test execution results in Test Runs. Test runs can be either manual or automated. Automated test runs can only contain automated tests \(Test Cases marked as "automated"\) while manual Test Runs can contain any Test Cases. The error message is provided by ADO when a non-automated Test Case is being added to the Test Run.

SpecSync sets both the Test Run and the Test Case automation status base on the configuration setting [`synchronization/automation/enabled`](../reference/configuration/configuration-synchronization/configuration-synchronization-automation.md). This means that if the same scenarios were synchronized by SpecSync then the ones that were executed in the test result file, such conflict cannot happen.

Cause: It can happen that some scenarios are excluded from the "automated" status using the [`synchronization/automation/skipForTags`](../reference/configuration/configuration-synchronization/configuration-synchronization-automation.md) configuration setting, but they were still included in the test result. 

**Solution 1:** Make sure that the Test Cases the test result belongs to have been successfully synchronized before publishing the test results with the same automation setting.

**Solution 2**: Try excluding the non-automated scenarios from the test execution

**Solution 3**: Create a separate SpecSync config file for test result publishing that sets `synchronization/automation/enabled` to `false`. \(You can use the [`toolSettings/parentConfig`](../reference/configuration/configuration-toolsettings.md) setting to make an [inherited configuration file](../features/general-features/hierarchical-configuration-files.md), so that you don't need to duplicate all configuration setting.\)

### "VS30063: You are not authorized to access" error when using SpecSync with Personal Access Tokens \(PAT\)

**Solution:** Please check if the PAT has all necessary permissions that are required to manage Test Cases and Test Suites.

### Invalid build ID detected when publish-test-result command is invoked from a CI/CD pipeline

SpecSync can associate the Test Run created by the publish-test-result command with an Azure DevOps build. This can be done manually, using the `--buildId` option, but SpecSync can also automatically detect the ID of the currently executing build from the `BUILD_BUILDID` environment variable. 

In some cases the environment variable does not contain a valid build ID or the build is not accessible for the user who performs the synchronization \(e.g. is in another Azure DevOps project\). In some cases the build ID is detected incorrectly from release pipelines.

**Solution 1:** Specify the build ID or the build number manually using the `--buildId` or the `--buildNumber` option \( e.g. `--buildId "$(parameter-that-contains-your-build-id)"`\). Note that the build ID is always a number. This number is shown in the URL when you open the build details. 

**Solution 2:** To avoid the automatic detection of a \(wrong\) build ID, set the `--buildId` option to an empty value \(  `--buildId " "`\). \(Some Azure DevOps build tasks do not handle empty arguments, therefore setting it to `""` might cause parameter errors.\)

