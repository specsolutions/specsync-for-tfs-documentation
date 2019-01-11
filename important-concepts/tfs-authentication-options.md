# TFS authentication options

SpecSync supports several authentication options for the [supported TFS systems](../compatibility.md#supported-tfs-systems). This section provides a summary of how the different authentication options have to be configured:

For Azure DevOps \(Visual Studio Team Services, VSTS\)

* [Personal access tokens](tfs-authentication-options.md#vsts-personal-access-tokens)
* [Alternate authentication credentials](tfs-authentication-options.md#vsts-alternate-authentication-credentials)
* [Visual Studio sign-in prompt](tfs-authentication-options.md#vsts-visual-studio-sign-in-prompt)

For on premises Team Foundation Server

* [Domain user name and password](tfs-authentication-options.md#tfs-domain-user-name-and-password)
* [Windows sign-in prompt](tfs-authentication-options.md#tfs-windows-sign-in-prompt)
* [Personal access tokens](tfs-authentication-options.md#vsts-personal-access-tokens)

The authentication credentials can be specified in the in the configuration file \(see [`remote` Configuration](../configuration/configuration-remote.md) for details\) or as command line options \(see [Usage](../usage.md) for details\).

For all configuration options, you can use environment variables for specifying user name and password \(`%variable%` format\), to make the testing easier without checking-in passwords to the source control. Environment variables are also useful for specifying passwords for [invoking the synchronization from the CI build process](synchronizing-test-cases-from-build.md).

The the environment variables can be used in the [configuration file](../configuration/configuration-remote.md):

```text
{
  ...
 "remote": {
    "tfsProjectUrl": "https://dev.azure.com/myorganization/MyProject",
    "user": "%VSO_USER%",
    "password": "%VSO_PWD%"
  },
  ...
}
```

And also in the command line prompt:

`path-to-specsync-package/tools/SpecSync4TFS.exe push --user "%%VSO_USER%%" --password "%%VSO_PWD%%"`

\(From shell scripts on Windows you have to use `%%` to avoid resolving the variable by the script itself.\)

## Personal access tokens <a id="vsts-personal-access-tokens"></a>

The recommended way to access your Azure DevOps project for synchronization is to use [personal access tokens](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=vsts) \(PAT\). PAT is like a combination of a user name and a password that are valid for a certain time only and can have restricted access to your TFS resources.

For on premises Team Foundation Server, PAT can only be used if the connection to the TFS server is using `https`.

To create a personal access token, you have to follow the following steps:

1. Navigate to your TFS server \(e.g. [https://myaccount.visualstudio.com](https://myaccount.visualstudio.com)\) with a browser.
2. On the upper right corner, click on your name and select "Security".
3. Switch to the "Personal access tokens" tab inside the "Security" group.
4. Click on "New Token" to create a new personal access token for synchronization.
5. Specify a description \(e.g. "SpecSync"\), select an expiration and select "Full access" as authorized scopes \(we will refine the necessary scopes in further versions\).
6. Click on "Create" and save the generated token.

Once you have created your token, you can use it as _user_ for the synchronization.

The personal access token can be configured in the [configuration file](../configuration/configuration-remote.md):

```text
{
  ...
 "remote": {
    "tfsProjectUrl": "https://dev.azure.com/myorganization/MyProject",
    "user": "52yny...........................nycsetda"
  },
  ...
}
```

Or on the command line prompt:

`path-to-specsync-package/tools/SpecSync4TFS.exe push --user "52yny...........................nycsetda"`

## Alternate authentication credentials <a id="vsts-alternate-authentication-credentials"></a>

A slightly less secure alternative of personal access tokens is to use alternate authentication credentials with Azure DevOps. With this option, you can provide a username/password pair that can be used for password-based authentication.

To enable alternate authentication credentials, you have to follow the following steps:

1. Navigate to your TFS server \(e.g. [https://myaccount.visualstudio.com](https://myaccount.visualstudio.com)\) with a browser.
2. On the upper right corner, click on your name and select "Security".
3. Switch to the "Alternate credentials" tab inside the "Security" group.
4. Check the "Enable alternate authentication credentials" checkbox and specify a \(secondary\) user name and password.
5. Click on "Save".

Once you have enabled alternate authentication credentials, you can use the synchronization with the secondary user name and password. This user name and password is not the same as the user name \(email\) and password you specify to login to Azure DevOps with your browser!

The alternate authentication credentials can be configured in the [configuration file](../configuration/configuration-remote.md):

```text
{
  ...
 "remote": {
    "tfsProjectUrl": "https://dev.azure.com/myorganization/MyProject",
    "user": "myalternateusername",
    "password": "myalternatepassword"
  },
  ...
}
```

Or on the command line prompt:

`path-to-specsync-package/tools/SpecSync4TFS.exe push --user myalternateusername --password myalternatepassword`

\(If you don't specify the `--password` option, the tool will prompt you for entering the password.\)

## Microsoft account sign-in prompt <a id="vsts-visual-studio-sign-in-prompt"></a>

For interactive sessions you can also provide your Azure DevOps credentials using the Microsoft account sign-in prompt \(similar to what you get when providing your credentials for Visual Studio\).

To authenticate using the sign-in prompt, you should _not_ specify the user.

```text
{
  ...
 "remote": {
    "tfsProjectUrl": "https://dev.azure.com/myorganization/MyProject"
  },
  ...
}
```

In the popup window you have to provide the user name \(email\) and the password you usually use for accessing Azure DevOps with the browser.

## Domain user name and password <a id="tfs-domain-user-name-and-password"></a>

For installed Team Foundation Servers, you can use your domain user name \(`MYDOMAIN\myuser` format\) and password for the synchronization.

The domain user name and password can be configured in the [configuration file](../configuration/configuration-remote.md):

```text
{
  ...
 "remote": {
    "tfsProjectUrl": "https://dev.azure.com/myorganization/MyProject",
    "user": "MYDOMAIN\myuser",
    "password": "mydomainpassword"
  },
  ...
}
```

Or on the command line prompt:

`path-to-specsync-package/tools/SpecSync4TFS.exe push --user mydomainpassword --password mydomainpassword`

\(If you don't specify the `--password` option, the tool will prompt you for entering the password.\)

## Windows sign-in prompt <a id="tfs-windows-sign-in-prompt"></a>

For installed Team Foundation Servers, if you don't specify the user, you will be asked to provide your credentials in an interactive sign-in prompt.

```text
{
  ...
 "remote": {
    "tfsProjectUrl": "http://mytfs:8080/tfs/DefaultCollection/MyProject"
  },
  ...
}
```

