# Troubleshooting

This page contains common issues and their solution. To better understand the synchronization process that led to the error, you can try to re-run the synchronization with an additional `--verbose` option.

If you don't find your issues here, please [contact support](specsync-support.md).

### Connecting to Azure DevOps fails with "API resource location 603fe2ac-9723-48b9-88ad-09305aa6c6e1 is not&#xD; registered" error

The Azure DevOps Connection might fail for various reasons, including incorrect [credentials](../features/general-features/server-authentication-options.md), missing permissions or [incompatible Azure DevOps versions](../reference/compatibility.md).&#x20;

If the error `API resource location 603fe2ac-9723-48b9-88ad-09305aa6c6e1 is not
 registered` is displayed (the ID might be different), the problem might be with the Azure DevOps project URL you have specified.

**Solution 1:** If you use TFS 2013 or TFS 2015, please check if the TFS server is compatible with the SpecSync version you use in the [Compatibility](../reference/compatibility.md) page. For older TFS servers you might need to donwgrade to an older version of SpecSync.

**Solution 2:** Check is the Azure DevOps project URL is correct. It can happen that the URL you have specified points to the project collection and the project name part is missing. You can use the [What is my Azure DevOps project URL](../important-concepts/what-is-my-server-url.md) guide for help. Fix the project URL and re-run the synchronization.

### Connecting to Azure DevOps with SpecSync v2 fails with "Unable to read data from the transport connection" error

The Azure DevOps team [deprecated TLS 1.0/1.1 access for Azure DevOps Services](https://devblogs.microsoft.com/devops/deprecating-weak-cryptographic-standards-tls-1-0-and-1-1-in-azure-devops-services/) (cloud version). As the Azure DevOps client library SpecSync uses for v2.1 or earlier uses TLS 1.1 by default you will get an error like:

```
Unable to authenticate to the Azure DevOps server.
  Underlying errors:
    And error occurred while sending the request.
    The underlying connection was closed: An unexpected error occurred on a send.
    Unable to read data from the transport connection: An existing connection was forcibly closed by the remote host.
    An existing connection was forcibly closed by the remote host.
```

**Solution 1:** To be able to use SpecSync v2.1 and earlier versions with Azure DevOps Services, you need to set the `remote/securityProtocol` setting to `Tls12`.

**Solution 2:** Upgrade to SpecSync v3 that uses the supported TLS 1.2 protocol by default.


### "VS30063: You are not authorized to access" error when using SpecSync with Personal Access Tokens (PAT)

**Solution:** Please check if the PAT is not expired and has all necessary permissions that are required to manage Test Cases and Test Suites. The required authorization scopes are listed on the [Azure DevOps authentication options](../features/general-features/server-authentication-options.md#authorization-scopes-required-for-personal-access-tokens) page.

### "GSSAPI operation failed with error - Unspecified GSS failure.  Minor code may provide more information (SPNEGO cannot find mechanisms to negotiate)" error when using SpecSync with Personal Access Tokens (PAT)

**Solution:** Please check if the PAT is not expired and has all necessary permissions that are required to manage Test Cases and Test Suites. The required authorization scopes are listed on the [Azure DevOps authentication options](../features/general-features/server-authentication-options.md#authorization-scopes-required-for-personal-access-tokens) page.

### Test result publishing fails with "Mismatch in automation status of test case and test run"

Azure DevOps (ADO) collects test execution results in Test Runs. Test runs can be either manual or automated. Automated test runs can only contain automated tests (Test Cases marked as "automated") while manual Test Runs can contain any Test Cases. The error message is provided by ADO when a non-automated Test Case is being added to the Test Run.

By default SpecSync sets both the Test Run and the Test Case automation status base on the configuration setting [`synchronization/automation/enabled`](../reference/configuration/configuration-synchronization/configuration-synchronization-automation.md). (see [Mark Test Cases as Automated](../features/push-features/mark-test-cases-as-automated.md)) This means that if the same scenarios were synchronized by SpecSync then the ones that were executed in the test result file, such conflict cannot happen.

Cause: It can happen that some scenarios are excluded from the "automated" status using the [`synchronization/automation/condition`](../reference/configuration/configuration-synchronization/configuration-synchronization-automation.md) or [`synchronization/automation/skipForTags`](../reference/configuration/configuration-synchronization/configuration-synchronization-automation.md)configuration setting, but they were still included in the test result.&#x20;

**Solution 1:** Make sure that the Test Cases the test result belongs to have been successfully synchronized before publishing the test results with the same automation setting.

**Solution 2:** Try excluding the non-automated scenarios from the test execution

**Solution 3:** Override the run type setting of the created Test Run using the `publishTestResults/testRunSettings/runType` configuration setting. This setting is available from SpecSync v3.1. For earlier SpecSync versions, create a separate SpecSync config file for test result publishing that sets `synchronization/automation/enabled` to `false`. (You can use the [`toolSettings/parentConfig`](../reference/configuration/configuration-toolsettings.md) setting to make an [inherited configuration file](../features/general-features/hierarchical-configuration-files.md), so that you don't need to duplicate all configuration setting.)

### Invalid build ID detected when publish-test-results command is invoked from a CI/CD pipeline

SpecSync can associate the Test Run created by the publish-test-results command with an Azure DevOps build. This can be done manually, using the `--buildId` option, but SpecSync can also automatically detect the ID of the currently executing build from the `BUILD_BUILDID` environment variable.&#x20;

In some cases the environment variable does not contain a valid build ID or the build is not accessible for the user who performs the synchronization (e.g. is in another Azure DevOps project). In some cases the build ID is detected incorrectly from release pipelines.

**Solution 1:** Specify the build ID or the build number manually using the `--buildId` or the `--buildNumber` option ( e.g. `--buildId "$(parameter-that-contains-your-build-id)"`). Note that the build ID is always a number. This number is shown in the URL when you open the build details.&#x20;

**Solution 2:** To avoid the automatic detection of a (wrong) build ID, use the `--disablePipelineAssociation` option (or set the `--buildId` option to an empty value before v3.3.3).

### Authentication/SSL error "The remote certificate is invalid according to the validation procedure." when connecting to an Azure DevOps Server (on promises)

Many on-promises installed Azure DevOps or TFS Server runs with self-signed or internally signed certificates. These certificates are usually installed on the machines of the team members using Windows Active Directory policies, so people might not even realize. In some cases though, especially when SpecSync is executed from Docker container, you might get an authentication error, similar to this:

```
Connection or authentication to server failed.
ERROR: Unable to authenticate to the Azure DevOps server.
  Underlying errors:
    The SSL connection could not be established, see inner exception.
    The remote certificate is invalid according to the validation procedure.
```

{% hint style="warning" %}
**Important note for all solutions below:** Declaring a certificate as trusted manually is a potential security risk, because by trusting a certificate created by a malicious site operator, they might be able to access the authentication credentials you use for connecting to Azure DevOps.

**Always make sure that the certificate you trust is coming from a trustworthy source.** The URL displayed in your browser alone does not guarantee trustworthiness. Ask help from your Azure DevOps operator in case of doubt.
{% endhint %}

**Solution 1:** To be able to use SpecSync with these services, you need to install the certificate.&#x20;

On Windows you can view and install the certificate from your browser, usually by clicking on the lock icon next to the URL (you should install the certificate to the "Trusted Root Certification Authorities" store of the current user).&#x20;

When the error comes from a Docker container, you either have to install the certificate to the Docker container or create a derived Docker Image that has this certificate installed. Installing the certificate on a docker container may vary with the Linux distribution used for the image, but with the most common setup, it requires the following steps

1. Export the certificate from your browser to a CER file (using DER encoding).
2. In the docker container, convert the CER file to CRT, for example using:\
   `openssl x509 -inform DER -in my-certificate.cer -out my-certificate.crt`
3. Copy the CRT file to the `/usr/local/share/ca-certificates/` folder of the container
4. Invoke `update-ca-certificates`

{% hint style="info" %}
The default certificate validation in Linux seems to be more strict than on Windows, so it can happen that a certificate that works after installing it on Windows might not work in Docker/Linux even after performing the setup instructions above. (E.g. when the certificate has an invalid "Key Usage" setting. Invalid settings are also displayed on Windows among the certificate details with a small exclamation mark.)

In this case you should contact your system administrators to generate a valid certificate or use Solution 2.
{% endhint %}

**Solution 2:** From version v3.1, SpecSync provides a setting to declare a particular certificate as trusted by specifying its thumbprint. It will only accept that particular certificate and for all the others, it uses the standard validation procedure.

The feature can be used by specifying the&#x20;
`-ignoreCertificateErrorsForThumbprint "<thumbprint>"`  [command line option](../reference/command-line-reference/) or setting the `"ignoreCertificateErrorsForThumbprint"` setting in the [`"remote"`](../reference/configuration/configuration-remote.md) section of the [configuration](../reference/configuration/).

The thumbprint can be found by checking the certificate from windows but it is also displayed by SpecSync when there was a certificate error.

We recommend using this solution only if installing the certificate on operating system level (Solution 1) is not possible.

### Build or release pipeline cannot be found error when publishing test results <a href="pipeline-not-found" id="pipeline-not-found"></a>

SpecSync automatically associate the build or release pipeline to the published test results if you invoke the `publish-test-results` command from a pipeline. When the pipeline is in a different Azure DevOps project than the synchronized Test Cases, you will get an error `Build <build-id> cannot be found.` or `Release environment with release uri <uri> and environment uri <uri> cannot be found.`. 

The error is caused by a limitation of Azure DevOps: The Test Run (this is the container that holds the test results) can only be associated to a build or release pipeline if that is in the same Azure DevOps project.

**Solution 1:** If possible, try to arrange your pipelines in a way that they belong to the same Azure DevOps project. (For large projects you can consider introducing [multiple teams](https://docs.microsoft.com/en-us/azure/devops/organizations/settings/about-teams-and-settings?view=azure-devops).)

**Solution 2:** If you cannot change the Azure DevOps project structure, you need do [disable the build association](../features/test-result-publishing-features/publishing-test-result-files.md#test-results-can-be-associated-to-an-azure-devops-build). This you can do by specifying the `--disablePipelineAssociation` option (or set the `--buildId` option to an empty value before v3.3.3).

In order to keep a documentation of the pipeline that performed the test execution, you can use the `--runComment` or the `--testResultComment` command line option. The test result comment is visible in all individual test results, the run comment is only visible on the Test Run of the test results.

For example to document the build number, you can specify: `--testResultComment "Build: $(Build.BuildNumber)"`. For documenting the release name, you can specify: `--testResultComment "Release: $(Release.ReleaseName)"`.

### 'You are not authrized to access this API' error when creating Test Cases or when adding them to Test Suite <a href="ado-user-access-level" id="ado-user-access-level"></a>

Azure DevOps uses different *user access lavels*. The default access level is *Basic* that does not include features required for maintaining Test Suites and creating Test Cases. To be able to use these features you need an access level that supports that, e.g. *Basic + Test Plans". You can find more information about the Azure DevOps user access levels required for working with Test Cases in the [Azure DevOps documentation](https://learn.microsoft.com/en-us/azure/devops/organizations/security/change-access-levels?view=azure-devops-2022).

For some functions, e.g. updating an *existing* Test Case work also with the *Basic* access level, you the error is shown only when you synchronize new scenarios or make any changes that need to modify the Test Suites (e.g. remove a scenario).

**Solution 1:** Change the user access level of the user you use for syncrhonization or synchronize the scenarios using another user that has the necessary access level. See [Azure DevOps documentation](https://learn.microsoft.com/en-us/azure/devops/organizations/security/change-access-levels?view=azure-devops-2022) for details.

**Solution 2:** Temporarily or for the period until the access level change is performed, you can ask other users to create Test Cases manually and you can add the link tags (e.g. `@tc:1234`) of these Test Cases to the scenarios manually. You also need to add the Test Cases to the Test Suite that is configured for SpecSync manually or diable the Test Suite synchronization feature of SpecSync by removing the `remote/testSuite` section.


### Unable to find or restore package error when installing or restoring SpecSync as a .NET tool <a href="nuget-package-not-found" id="nuget-package-not-found"></a>

When you install SpecSync as a [.NET tool](../installation/dotnet-core-tool.md), you might receive errors during the `dotnet tool install SpecSync.AzureDevOps` or the `dotnet tool restore` commands. The error message might contain `Unable to find package SpecSync.AzureDevOps` or `The tool package could not be restored`.

The problem is usually that there is a custom NuGet source configured in your organization that does not mirror SpecSync packages.

**Solution 1:** For the installation you can specify the official NuGet source by adding the  `--add-source  https://api.nuget.org/v3/index.json --ignore-failed-sources` options to the install or restore command. 

E.g. the installation with specified NuGet source would be...

```bash
dotnet tool install SpecSync.AzureDevOps --add-source  https://api.nuget.org/v3/index.json --ignore-failed-sources
```

**Solution 2:** You can change your local NuGet configuration or the `NuGet.config` file of your project to include the official NuGet source `https://api.nuget.org/v3/index.json`. Alternatively you can ask the administrator of your custom NuGet source to mirror the SpecSync NuGet packages.

### No usable version of libssl was found error when running SpecSync Linux binaries on Ubuntu 22.04 <a href="no-usable-libssl-ubuntu2204" id="no-usable-libssl-ubuntu2204"></a>

When the Linux binary package of an older SpecSync version is executed on Ubuntu 22.04 the synchronization might fail with the error message `No usable version of libssl was found`.

This is caused by the SSL library incompatibility between .NET Core 3.1 and Ubuntu 22.04.

**Solution 1:** You can use the Ubuntu 20.04 stable version for these SpecSync versions where you can have compatible SSL packages (`libssl1.1`).

**Solution 2:** Upgrade SpecSync for Azure DevOps to v3.4. Those versions use .NET 6 for the Linux binaries that are compatible with `libssl3` available for Ubuntu 22.04.

**Solution 3:** The compatibility can also be achieved by downgrading and installing the necessary libssl library. This can be achieved by adding the following command to your `Dockerfile` (source: [StackOverflow post](https://stackoverflow.com/a/72137153/26530)):

```text
RUN wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl1.0/libssl1.0.0_1.0.2n-1ubuntu5_amd64.deb \
    && dpkg -i libssl1.0.0_1.0.2n-1ubuntu5_amd64.deb
```

### Unhandled JsonSerializationException error when publishing test results using multi-suite publish <a href="issue1357" id="issue1357"></a>

On 17th October 2024 we have detected and analyzed a problem that causes SpecSync to fail with an unhandled JsonSerializationException during the "publish-test-results" command when the "multiSuitePublishTestResults" customization is used. The investigation found that the issue is caused by a change in Azure DevOps that is not compatible with the Azure DevOps API client library. We have [reported the issue to Microsoft](https://developercommunity.visualstudio.com/t/JsonSerializationException-when-calling/10771178) which is currently under investigation by the Azure DevOps team. As most probably the fix will take longer, we implemented an alternative solution in SpecSync, that is not affected by the issue. 

For SpecSync the issue has been registered with the ID `#1357`.

**Solution 1:** Upgrade to SpecSync v3.4.18 or later. (For users on v3.3 product line, please upgrade to v3.3.17.)

**Solution 2:** Do not use the "multiSuitePublishTestResults" customization until the issue is fixed in Azure DevOps. (Please track the issue at the link provided above.)

### Test execution results are not visible in all Test Suites even though "Show same outcome of the tests in multiple suites under the same plan" is enabled

When a Test Case exists in multiple Test Suites within the same Test Plan, you might expect that when the test execution results are published for that Test Case, the results will automatically appear in all Test Suites where the Test Case exists.

Azure DevOps provides a Test Plan setting named "Show same outcome of the tests in multiple suites under the same plan" which sounds like it might enable this behavior. However, this Azure DevOps feature currently only works for manually executed tests and cannot be used with automated test execution results published by SpecSync.

This is a limitation of Azure DevOps and there is no workaround available from Azure DevOps side. You can track the issue and request improvements on the [Azure DevOps Developer Community](https://developercommunity.visualstudio.com/t/show-same-outcome-of-the-tests-in-multiple-suites/1546796).

**Solution 1:** With SpecSync v5 or later you can use the [hierarchies feature](../features/common-synchronization-features/synchronizing-test-case-hierarchies.md) to synchronize the Test Case hierarchies using Test Suites. When selecting a hierarchy for publishing test results (for example by using the `--hierarchy` option), SpecSync will ensure that the test execution results are published to all Test Suites in the selected hierarchy.

**Solution 2:** In older version of SpecSync, you can use the [multiSuitePublishTestResults customization](../features/test-result-publishing-features/customization-publishing-test-results-to-multiple-test-suite.md) to publish the test execution results to multiple Test Suites.


### 'W5113: Unable to find attachment file' warnings when publishing results from TRX file <a href="issue1668" id="issue1668"></a>

When the TRX logger is used with `dotnet test` command for running the tests to be published with SpecSync, you might encounter warnings like

* `W5113: Unable to find attachment file '<some-attachment-path>'. Ignored.`

The issue is probably caused by the TRX logger, that saves the attachment files to the test output folder (usually the `TestResults` folder of the project), but the TRX file is saved elsewhere, for example because a folder is specified for the TRX logger in the `LogFileName` setting, like `--logger "trx;LogFileName=..\results.trx`.

**Solution:** Use the `--results-directory` setting of `dotnet test` to specify the results directory, and use the `LogFileName` only to specify the file name. E.g. `dotnet test --logger trx;logfilename=results.trx --results-directory "C:\MyProject"`.

