# Azure DevOps authentication options

SpecSync supports several authentication options for the [supported Azure DevOps systems](../../reference/compatibility.md#supported-server-systems). This section provides a summary of how the different authentication options have to be configured:

For Azure DevOps (Visual Studio Team Services, VSTS):

* [Personal access tokens](server-authentication-options.md#vsts-personal-access-tokens)
* [Job access token](server-authentication-options.md#vsts-alternate-authentication-credentials) (from build and release pipelines)
* [Alternate authentication credentials](server-authentication-options.md#vsts-alternate-authentication-credentials)
* [Microsoft account sign-in prompt](server-authentication-options.md#vsts-visual-studio-sign-in-prompt)

For on premises Azure DevOps Server (or Team Foundation Server):

* [Personal access tokens](server-authentication-options.md#vsts-personal-access-tokens)
* [Job access token](server-authentication-options.md#vsts-alternate-authentication-credentials) (from build and release pipelines)
* [Domain user name and password](server-authentication-options.md#tfs-domain-user-name-and-password)
* [Windows sign-in prompt](server-authentication-options.md#tfs-windows-sign-in-prompt)

The authentication credentials can be specified in multiple ways:

1. During the execution of the SpecSync command using the interactive prompt (password is masked). This method is used by SpecSync if either the user name (token) or the password is not configured anywhere else.
2. Using the `--user` and `--password` [command line options](../../reference/command-line-reference/#common-command-line-options).
3. In the [`remote` section](../../reference/configuration/configuration-remote.md) of the [SpecSync configuration file](configuration-file.md) (`specsync.json`).
4. In the [user-specific configuration file](hierarchical-configuration-files.md#user-specific-configuration-files).
5. In system environment variables that can be referred to either in the configuration file or from the command prompt (see examples below). The environment variable name has to be specified using the `%variable%` format.

### Examples

Specifying the user name in the `specsync.json` configuration file:

```
{
  ...
 "remote": {
    "projectUrl": "https://specsyncdemo.visualstudio.com/MyCalculator",
    "user": "account01"
  },
  ...
}
```

Specifying the user name in the command line:

```text
dotnet specsync push --user "account01"
```

A [user-specific configuration file](hierarchical-configuration-files.md#user-specific-configuration-files) that configures credentials for multiple projects.


{% code title="specsync.json (in C:\Users\my_user\AppData\SpecSync folder)" %}
```javascript
{
  "$schema": "http://schemas.specsolutions.eu/specsync-user-config-latest.json",

  "knownRemotes": [
    {
      "projectUrl": "https://dev.azure.com/myorg/*",
      "user": "g2x.....................ac2vx57i4a"
    },
    {
      "projectUrl": "https://dev.azure.com/otherorg/OtherProject",
      "user": "y6s.....................ksuc7tsts"
    }
  ]
}
```
{% endcode %}

Specifying the user credentials that refer to an environment variable in the `specsync.json` configuration file. The example requires the user name to be stored in the environment variable `SPECSYNC_REMOTE_USER`:

```
{
  ...
 "remote": {
    "projectUrl": "https://specsyncdemo.visualstudio.com/MyCalculator",
    "user": "%SPECSYNC_REMOTE_USER%"
  },
  ...
}
```

You can also use the environment variables without the shell resolving their values. For that, specify the value in the `%variable%` format. From shell scripts on Windows you have to use `%%` to avoid resolving the variable by the script itself.

```text
dotnet specsync push --user "%%SPECSYNC_REMOTE_USER%%"
```


## Personal access tokens  <a href="vsts-personal-access-tokens" id="vsts-personal-access-tokens"></a>

The recommended way to access your Azure DevOps project for synchronization is to use [personal access tokens](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=vsts) (PAT). PAT is like a combination of a user name and a password that are valid for a certain time only and can have restricted access to your Azure DevOps resources.

For on premises Team Foundation Server, PAT can only be used if the connection to the Azure DevOps server is using `https`.

To create a personal access token, you have to follow the following steps:

1. Navigate to your Azure DevOps server (e.g. [https://myaccount.visualstudio.com](https://myaccount.visualstudio.com)) with a browser.
2. On the upper right corner, click on your name and select "Security".
3. Switch to the "Personal access tokens" tab inside the "Security" group.
4. Click on "New Token" to create a new personal access token for synchronization.
5. Specify a description (e.g. "SpecSync"), select an expiration and select "Full access" as authorized scopes or select at least the scopes listed below in section [Authorization scopes required for Personal Access Tokens](server-authentication-options.md#authorization-scopes-required-for-personal-access-tokens).
6. Click on "Create" and save the generated token.

Once you have created your token, you can use it as _user_ for the synchronization.

The personal access token can be configured in the [configuration file](../../reference/configuration/configuration-remote.md):

```
{
  ...
 "remote": {
    "projectUrl": "https://dev.azure.com/myorganization/MyProject",
    "user": "52yny...........................nycsetda"
  },
  ...
}
```

Or on the command line prompt:

`path-to-specsync-package/tools/SpecSync4AzureDevOps.exe push --user "52yny...........................nycsetda"`

### Authorization scopes required for Personal Access Tokens

In order to work correctly SpecSync requires at least the following authorization scopes enabled for Personal Access Tokens (PAT). The Build and Release read permissions are required to associate the test results to build and release pipelines.

| PAT Scope       | Required permission |
| --------------- | ------------------- |
| Work Items      | Read & write        |
| Test Management | Read & write        |
| Build           | Read                |
| Release         | Read                |

## Job access token <a href="vsts-alternate-authentication-credentials" id="vsts-alternate-authentication-credentials"></a>

When performing synchronization from build and release pipelines the easiest is to use the Job access token (sometimes also referred as System access token). A job access token is a security token that is dynamically generated by Azure Pipelines for each job at run time. The agent on which the job is running can use the job access token in order to access resources in Azure DevOps. Learn more about job access tokens in the [Azure DevOps documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/access-tokens?view=azure-devops\&tabs=yaml).

{% hint style="info" %}
To be able to use the Job access token, you need to enable it for the pipeline, by selecting the agent job and in the _Additional options_ section, enable the setting _Allow scripts to access the OAuth token_.
{% endhint %}

Once the Job access token is enabled, you can refer to it with `$(System.AccessToken)`. E.g.

```
push --user "$(System.AccessToken)"
```

You can find more information about configuring a build or release pipeline for running synchronization in [synchronizing-test-cases-from-build.md](../../important-concepts/synchronizing-test-cases-from-build.md "mention").

## Alternate authentication credentials  <a href="vsts-alternate-authentication-credentials" id="vsts-alternate-authentication-credentials"></a>

{% hint style="warning" %}
Azure DevOps no longer supports alternate credentials since 2020. It is recommended to use [Personal Access Tokens](server-authentication-options.md#vsts-personal-access-tokens) instead. See [Microsoft announcement](https://devblogs.microsoft.com/devops/azure-devops-will-no-longer-support-alternate-credentials-authentication) for details.
{% endhint %}

A slightly less secure alternative of personal access tokens is to use alternate authentication credentials with Azure DevOps. With this option, you can provide a username/password pair that can be used for password-based authentication.

To enable alternate authentication credentials, you have to follow the following steps:

1. Navigate to your Azure DevOps server (e.g. [https://myaccount.visualstudio.com](https://myaccount.visualstudio.com)) with a browser.
2. On the upper right corner, click on your name and select "Security".
3. Switch to the "Alternate credentials" tab inside the "Security" group.
4. Check the "Enable alternate authentication credentials" checkbox and specify a (secondary) user name and password.
5. Click on "Save".

Once you have enabled alternate authentication credentials, you can use the synchronization with the secondary user name and password. This user name and password is not the same as the user name (email) and password you specify to login to Azure DevOps with your browser!

The alternate authentication credentials can be configured in the [configuration file](../../reference/configuration/configuration-remote.md):

```
{
  ...
 "remote": {
    "projectUrl": "https://dev.azure.com/myorganization/MyProject",
    "user": "myalternateusername",
    "password": "myalternatepassword"
  },
  ...
}
```

Or on the command line prompt:

`path-to-specsync-package/tools/SpecSync4AzureDevOps.exe push --user myalternateusername --password myalternatepassword`

(If you don't specify the `--password` option, the tool will prompt you for entering the password.)

## Microsoft account sign-in prompt  <a href="vsts-visual-studio-sign-in-prompt" id="vsts-visual-studio-sign-in-prompt"></a>

{% hint style="warning" %}
The Microsoft account sign-in prompt is currently not supported when you use the [.NET Core or the native binary installation](../../installation/) of SpecSync.
{% endhint %}

For interactive sessions you can also provide your Azure DevOps credentials using the Microsoft account sign-in prompt (similar to what you get when providing your credentials for Visual Studio).

To authenticate using the sign-in prompt, you should _not_ specify the user.

```
{
  ...
 "remote": {
    "projectUrl": "https://dev.azure.com/myorganization/MyProject"
  },
  ...
}
```

In the popup window you have to provide the user name (email) and the password you usually use for accessing Azure DevOps with the browser.

## Domain user name and password  <a href="tfs-domain-user-name-and-password" id="tfs-domain-user-name-and-password"></a>

For installed Team Foundation Servers, you can use your domain user name (`MYDOMAIN\myuser` format) and password for the synchronization.

The domain user name and password can be configured in the [configuration file](../../reference/configuration/configuration-remote.md):

```
{
  ...
 "remote": {
    "projectUrl": "https://dev.azure.com/myorganization/MyProject",
    "user": "MYDOMAIN\myuser",
    "password": "mydomainpassword"
  },
  ...
}
```

Or on the command line prompt:

`path-to-specsync-package/tools/SpecSync4AzureDevOps.exe push --user mydomainpassword --password mydomainpassword`

(If you don't specify the `--password` option, the tool will prompt you for entering the password.)

## Windows sign-in prompt  <a href="tfs-windows-sign-in-prompt" id="tfs-windows-sign-in-prompt"></a>

{% hint style="warning" %}
The Windows sign-in prompt is currently not supported when you use the [.NET Core or the native binary installation](../../installation/) of SpecSync.
{% endhint %}

For installed Team Foundation Servers, if you don't specify the user, you will be asked to provide your credentials in an interactive sign-in prompt.

```
{
  ...
 "remote": {
    "projectUrl": "http://mytfs:8080/tfs/DefaultCollection/MyProject"
  },
  ...
}
```
