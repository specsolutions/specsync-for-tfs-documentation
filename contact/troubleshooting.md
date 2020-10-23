# Troubleshooting

This page contains common issues and their solution. To better understand the synchronization process that led to the error, you can try to re-run the synchronization with an additional `--verbose` option.

If you don't find your issues here, please [contact support](specsync-support.md).

### Connecting to Azure DevOps fails with "API resource location 603fe2ac-9723-48b9-88ad-09305aa6c6e1 is not registered" error

The Azure DevOps Connection might fail for various reasons, including incorrect [credentials](../features/general-features/tfs-authentication-options.md), missing permissions or [incompatible Azure DevOps versions](../reference/compatibility.md). 

If the error `API resource location 603fe2ac-9723-48b9-88ad-09305aa6c6e1 is not registered` is displayed \(the ID might be different\), the problem might be with the Azure DevOps project URL you have specified.

**Solution 1:** If you use TFS 2013 or TFS 2015, please check if the TFS server is compatible with the SpecSync version you use in the [Compatibility](../reference/compatibility.md) page. For older TFS servers you might need to donwgrade to an older version of SpecSync.

**Solution 2:** Check is the Azure DevOps project URL is correct. It can happen that the URL you have specified points to the project collection and the project name part is missing. You can use the [What is my Azure DevOps project URL](../important-concepts/what-is-my-tfs-project-url.md) guide for help. Fix the project URL and rerun the synchronization.

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

### Authentication/SSL error "The remote certificate is invalid according to the validation procedure." when connecting to an Azure DevOps Server \(on promises\)

Many on-promises installed Azure DevOps or TFS Server runs with self-signed or internally signed certificates. These certificates are usually installed on the machines of the team members using Windows Active Directory policies, so people might not even realize. In some cases though, especially when SpecSync is executed from Docker container, you might get an authentication error, similar to this:

```text
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

**Solution 1:** To be able to use SpecSync with these services, you need to install the certificate. 

On Windows you can view and install the certificate from your browser, usually by clicking on the lock icon next to the URL \(you should install the certificate to the "Trusted Root Certification Authorities" store of the current user\). 

When the error comes from a Docker container, you either have to install the certificate to the Docker container or create a derived Docker Image that has this certificate installed. Installing the certificate on a docker container may vary with the Linux distribution used for the image, but with the most common setup, it requires the following steps

1. Export the certificate from your browser to a CER file \(using DER encoding\).
2. In the docker container, convert the CER file to CRT, for example using: `openssl x509 -inform DER -in my-certificate.cer -out my-certificate.crt`
3. Copy the CRT file to the `/usr/local/share/ca-certificates/` folder of the container
4. Invoke `update-ca-certificates`

{% hint style="info" %}
The default certificate validation in Linux seems to be more strict than on Windows, so it can happen that a certificate that works after installing it on Windows might not work in Docker/Linux even after performing the setup instructions above. \(E.g. when the certificate has an invalid "Key Usage" setting. Invalid settings are also displayed on Windows among the certificate details with a small exclamation mark.\)

In this case you should contact your system administrators to generate a valid certificate or use Solution 2.
{% endhint %}

**Solution 2:** From version v3.1, SpecSync provides a setting to declare a particular certificate as trusted by specifying its thumbprint. It will only accept that particular certificate and for all the others, it uses the standard validation procedure.

The feature can be used by specifying the `-ignoreCertificateErrorsForThumbprint "<thumbprint>"`  [command line option](../reference/command-line-reference/) or setting the `"ignoreCertificateErrorsForThumbprint"` setting in the [`"remote"`](../reference/configuration/configuration-remote.md) section of the [configuration](../reference/configuration/).

The thumbprint can be found by checking the certificate from windows but it is also displayed by SpecSync when there was a certificate error.

We recommend using this solution only if installing the certificate on operating system level \(Solution 1\) is not possible.

