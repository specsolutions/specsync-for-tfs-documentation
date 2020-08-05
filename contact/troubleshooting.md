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

