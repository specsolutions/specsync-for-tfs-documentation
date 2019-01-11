# `remote` Configuration

This configuration section contains settings for accessing the test cases on the remote TFS server.

The following example shows the available options within this section.

```
{
  ...
 "remote": {
    "tfsProjectUrl": "https://dev.azure.com/myorganization/MyProject",
    "user": "myuser",
    "password": "%MYPWD%",
    "testSuite": {
      "name": "BDD Scenarios"
    }
  },
  ...
}
```

## Settings

* `tfsProjectUrl` -- The full URL of the TFS or VSTS project (including project collection name if necessary). Must not include project team name: for multi-team projects the root project URL has to be specified. See [What is my TFS project URL](what-is-my-tfs-project-url.md) for details. (Mandatory.)
* `user` -- The TFS user name or [personal access token](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=vsts) (PAT) to be used for authentication. It may contain environment variables in `...%MYENV%...` form. See [TFS authentication options](../tfs-authentication-options.md) for details. (Default: *[interactive prompt]*)
* `password` -- The password to be used for authentication. It may contain environment variables in `...%MYENV%...` form. See [TFS authentication options](../tfs-authentication-options.md) for details. (Default: *[interactive prompt]*)
* `testSuite` -- Specifies a test suite within the TFS project as a target container of the synchronized test cases. If the test suite is specified, SpecSync will add the synchronized test cases to it for `push` command and consider the test cases within the suite for `pull` command. See [Group synchronized test cases to a test suite](../group-synchronized-test-cases-to-a-test-suite.md) for details. (Default: *[test cases are not included to a test suite]*)
  * `testSuite/name` -- The name of the test suite. For suites with non-unique names, please use the `testSuite/id` setting.
  * `testSuite/id` -- "The ID of the test suite as a number (e.g. `id: 12345`)."

*[Back to the [Configuration guide](../configuration.md)]*