# remote

This configuration section contains settings for accessing the test cases on the remote Azure DevOps server.

The following example shows the most common options within this section.

```javascript
{
  ...
  "remote": {
    "projectUrl": "https://dev.azure.com/myorganization/MyProject",
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

<table>
  <thead>
    <tr>
      <th style="text-align:left">Setting</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>projectUrl</code>
      </td>
      <td style="text-align:left">The full URL of the Azure DevOps or VSTS project (including project collection
        name if necessary). Must not include project team name: for multi-team
        projects the root project URL has to be specified. See <a href="../../important-concepts/what-is-my-tfs-project-url.md">What is my Azure DevOps project URL</a> for
        details.</td>
      <td style="text-align:left">mandatory</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>user</code>
      </td>
      <td style="text-align:left">The Azure DevOps user name or <a href="https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=vsts">personal access token</a> (PAT)
        to be used for authentication. It may contain environment variables in <code>...%MYENV%...</code> form.
        See <a href="../../features/general-features/tfs-authentication-options.md">Azure DevOps authentication options</a> for
        details.</td>
      <td style="text-align:left">interactive prompt</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>password</code>
      </td>
      <td style="text-align:left">The password to be used for authentication. It may contain environment
        variables in <code>...%MYENV%...</code> form. See <a href="../../features/general-features/tfs-authentication-options.md">Azure DevOps authentication options</a> for
        details.</td>
      <td style="text-align:left">interactive prompt</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>securityProtocol</code>
      </td>
      <td style="text-align:left">The security protocol to be used for the remote connection (<code>Ssl3</code>, <code>Tls</code>, <code>Tls11</code>, <code>Tls12</code>).</td>
      <td
      style="text-align:left">system default</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>ignoreCertificateErrorsForThumbprint</code>
      </td>
      <td style="text-align:left">The thumbprint of the server certificate that should be treated as trusted.
        It is recommended to install trusted certificates on the operating system
        instead of using this setting. See related <a href="../../contact/troubleshooting.md#authentication-ssl-error-the-remote-certificate-is-invalid-according-to-the-validation-procedure-when-connecting-to-an-azure-devops-server-on-promises">Troubleshooting entry</a> for
        details.</td>
      <td style="text-align:left">SSL is verified by the OS</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>testSuite</code>
      </td>
      <td style="text-align:left">
        <p>Specifies a test suite within the Azure DevOps project as a target container
          of the synchronized test cases. If the test suite is specified, SpecSync
          will add the synchronized test cases to it for <code>push</code> command
          and consider the test cases within the suite for <code>pull</code> command.
          See <a href="../../features/common-synchronization-features/group-synchronized-test-cases-to-a-test-suite.md">Group synchronized test cases to a test suite</a> for
          details.</p>
        <ul>
          <li><code>testSuite/name</code> &#x2014; The name of the test suite. For suites
            with non-unique names, please use the <code>testSuite/id</code> setting.</li>
          <li><code>testSuite/id</code> &#x2014; The ID of the test suite as a number
            (e.g. <code>id: 12345</code>).</li>
          <li><code>testSuite/testPlanId</code> &#x2014;The ID of the test plan to search
            or create the test suite in. (Optional, improves performance)</li>
        </ul>
      </td>
      <td style="text-align:left">test cases are not included to a test suite</td>
    </tr>
  </tbody>
</table>

{% page-ref page="./" %}

