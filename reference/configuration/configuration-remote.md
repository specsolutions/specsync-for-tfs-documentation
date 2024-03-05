# remote

This configuration section contains settings for accessing the test cases on the remote Azure DevOps server.

The following example shows the most common options within this section.

```javascript
{
  ...
  "remote": {
    "projectUrl": "https://specsyncdemo.visualstudio.com/MyCalculator",
    "user": "myuser",
    "password": "%MYPWD%",
    "testSuite": {
      "name": "BDD Scenarios",
      "testPlanId": 12345
    }
  },
  ...
}
```

## Settings

{% hint style="info" %}
The remote settings of the Azure DevOps project can also be specified in the knownRemotes section of one of the parent configuration file or in the user-specific configuration file. Check [Hierarchical configuration files](../../features/general-features/hierarchical-configuration-files.md) for details.
{% endhint %}

| Setting | Description | Default |
| ------- | ----------- | ------- |
| `projectUrl` | The full URL of the Azure DevOps or VSTS project (including project collection name if necessary). Must not include project team name: for multi-team projects the root project URL has to be specified. See [What is my Azure DevOps project URL](../../important-concepts/what-is-my-server-url.md) for details. | mandatory |
| `authenticationMethod` | The authentication method to be used for connecting to the remote server. Possible values: `Interactive`, `UserNamePassword`, `PAT`, `ServicePrincipal` | automatically detect `UserNamePassword` or `PAT` |
| `user` | The Azure DevOps user name or [personal access token](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=vsts) (PAT) to be used for authentication. For `ServicePrincipal` authentication method the application ID is specified here. It may contain environment variables in `...{env:ENV_VAR}...` form. See [Azure DevOps authentication options](../../features/general-features/server-authentication-options.md) for details. | interactive prompt |
| `password` | The password to be used for authentication. For `ServicePrincipal` authentication method the client secret is specified here. It may contain environment variables in `...{env:ENV_VAR}...` form. See [Azure DevOps authentication options](../../features/general-features/server-authentication-options.md) for details. | interactive prompt |
| `tenantId` | Only for `ServicePrincipal` authentication method: specify the Microsoft Entra Tenant ID. | Mandatory for `ServicePrincipal` authentication method |
| `authenticationCertificateThumbprint` | Only for `ServicePrincipal` authentication method: specify the thumbprint of the client certificate to be used for authentication. The certificate has to be saved to the Personal certificate store. | client secret (`password`) is used |
| `securityProtocol` | The security protocol to be used for the remote connection (<code>Ssl3</code>, <code>Tls</code>, <code>Tls11</code>, <code>Tls12</code>). | system default |
| `ignoreCertificateErrorsForThumbprint` | The thumbprint of the server certificate that should be treated as trusted. It is recommended to install trusted certificates on the operating system instead of using this setting. See related [Troubleshooting entry](../../contact/troubleshooting.md#authentication-ssl-error-the-remote-certificate-is-invalid-according-to-the-validation-procedure-when-connecting-to-an-azure-devops-server-on-promises) for details. | SSL is verified by the OS |
| `testSuite` | Specifies a test suite within the Azure DevOps project as a target container of the synchronized test cases. If the test suite is specified, SpecSync will add the synchronized test cases to it for `push` command and consider the test cases within the suite for `pull` command. See [Group synchronized test cases to a test suite](../../features/common-synchronization-features/group-synchronized-test-cases-to-a-test-suite.md) for details. | test cases are not included in a test suite |
| `testSuite/name` | The name of the Test Suite. For suites with non-unique names, please use the `testSuite/id` or `testSuite/path` setting. | either `name`, `id` or `path` is mandatory |
| `testSuite/id` | The ID of the Test Suite as a number. | either `name`, `id` or `path` is mandatory |
| `testSuite/path` | The path of the Test Suite from the root of the Test Plan, separated by `/` (e.g. `Ordering/Card Payment`). | either `name`, `id` or `path` is mandatory |
| `testSuite/testPlanId` | Deprecated, use 'testPlan' instead. | not specified |
| `testSuite/testPlan` | The name or ID of the Test Plan to search or create the test suite in, e.g. `My Plan` or `#1234`. (Optional, improves performance) | not specified |
| `azureDevOps` | <p>Azure DevOps server configuration settings.</p><ul><li><code>testCaseWorkItemName</code> &#x2014; The name of the Test Case work item in a localized Azure DevOps process (Default: `Test Case`)</li><li><code>testSuiteWorkItemName</code> &#x2014; The name of the Test Suite work item in a localized Azure DevOps process (Default: `Test Suite`)</li></ul> | uses default AzureDevOps server settings |


{% page-ref page="./" %}

