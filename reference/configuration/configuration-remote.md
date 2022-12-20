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
| `user` | The Azure DevOps user name or [personal access token](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=vsts) (PAT) to be used for authentication. It may contain environment variables in <code>...%MYENV%...</code> form. See [Azure DevOps authentication options](../../features/general-features/server-authentication-options.md) for details. | interactive prompt |
| `password` | The password to be used for authentication. It may contain environment variables in <code>...%MYENV%...</code> form. See [Azure DevOps authentication options](../../features/general-features/server-authentication-options.md) for details. | interactive prompt |
| `securityProtocol` | The security protocol to be used for the remote connection (<code>Ssl3</code>, <code>Tls</code>, <code>Tls11</code>, <code>Tls12</code>). | system default |
| `ignoreCertificateErrorsForThumbprint` | The thumbprint of the server certificate that should be treated as trusted. It is recommended to install trusted certificates on the operating system instead of using this setting. See related [Troubleshooting entry](../../contact/troubleshooting.md#authentication-ssl-error-the-remote-certificate-is-invalid-according-to-the-validation-procedure-when-connecting-to-an-azure-devops-server-on-promises) for details. | SSL is verified by the OS |
| `testSuite` | <p>Specifies a test suite within the Azure DevOps project as a target container of the synchronized test cases. If the test suite is specified, SpecSync will add the synchronized test cases to it for <code>push</code> command and consider the test cases within the suite for <code>pull</code> command. See <a href="../../features/common-synchronization-features/group-synchronized-test-cases-to-a-test-suite.md">Group synchronized test cases to a test suite</a> for details.</p><ul><li><code>testSuite/name</code> &#x2014; The name of the test suite. For suites with non-unique names, please use the <code>testSuite/id</code> setting.</li><li><code>testSuite/id</code> &#x2014; The ID of the test suite as a number (e.g. <code>id: 12345</code>).</li><li><code>testSuite/testPlanId</code> &#x2014;The ID of the test plan to search or create the test suite in. (Optional, improves performance)</li></ul> | test cases are not included to a test suite |
| `azureDevOps` | <p>Azure DevOps server configuration settings.</p><ul><li><code>testCaseWorkItemName/name</code> &#x2014; The name of the Test Case work item in a localized Azure DevOps process (Default: `Test Case`)</li><li><code>testSuiteWorkItemName/id</code> &#x2014; The name of the Test Suite work item in a localized Azure DevOps process (Default: `Test Suite`)</li></ul> | uses default AzureDevOps server settings |


{% page-ref page="./" %}

