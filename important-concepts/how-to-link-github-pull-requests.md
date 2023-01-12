# How to link GitHub pull requests

Recent versions Azure DevOps support linking Test Cases to GitHub pull requests. You can configure SpecSync to establish such links using th [Link tags feature](../features/common-synchronization-features/linking-work-items-with-tags.md) and the [Link on change customization](../features/push-features/customization-automatically-link-changed-test-cases.md).

In order to use this feature you need to make sure that this feature is enabled in your Azure DevOps project and you can link GitHub pull requests manually via the Azure DevOps user interface. Please check the [Azure DevOps documentation](https://learn.microsoft.com/en-us/azure/devops/boards/github/link-to-from-github?view=azure-devops#add-link-from-a-work-item-to-a-github-commit-pull-request-or-issue) for details on how linking works.

If you cannot link GitHup pull requests manually, then the GitHub integration is not properly configured for your project. In essential, you need to connect the Azure DevOps project with the GitHub project using the Azure Boards-GitHub integration. See more details on the setup in the [Azure DevOps documentation](https://learn.microsoft.com/en-us/azure/devops/boards/github/?view=azure-devops) about this.

## Linking GitHub pull requests with tags

For linking a Test Case with a particular GitHub pull request (e.g. the with the pull request that introduced the test), you need to first configure a link prefix using the [Link tags feature](../features/common-synchronization-features/linking-work-items-with-tags.md).

The following example configures the tag prefix `GHPR`. The `relationship` setting has to be set to `GitHub Pull Request` and you need to provide the link template to your GitHub pull request. In the link template, you have to use `{id}` at the position where the pull request ID should be inserted.

```javascript
{
  ...
  "synchronization": {
    ...
    "links": [
      {
        "tagPrefix": "GHPR",
        "relationship": "GitHub Pull Request",
        "linkTemplate": "https://github.com/my-org/my-project/pull/{id}"
      }
    ],
    ...
  },
  ...
}
```

With the link configured, you can add tags to your scenarios in order to establish a link to the GitHub pull request. The following example shows how to link the scenario `Add two positive numbers` to the GitHub pull request #123 using the `GHPR` link prefix we configured above.

```text
@tc:1234
@GHPR:123
Scenario: Add two positive numbers
```

From SpecSync v3.4, you can also use the full pull request URL in the tag:

```text
@tc:1234
@GHPR:https://github.com/my-org/my-project/pull/123
Scenario: Add two positive numbers
```

## Linking GitHub pull requests when the test case changes

The [Link on change customization](../features/push-features/customization-automatically-link-changed-test-cases.md) can automatically establish links to the Test Cases that are changed during the current push command. This might be useful for CI/CD pipeline executions. See feature description for details.

The customization also works with GitHub pull requests: you can link the Test Cases to a GitHub pull request in your build pipeline. For that, you need to specify `GitHub Pull Request` as `relationship` and you have to specify the GitHub pull request link template for the `linkTemplate` setting. In the link template, you have to use `{id}` at the position where the pull request ID should be inserted.

The following customization links the changed Test Cases to the GitHub pull request set in the environment variable `ENVVAR_WITH_PR_NUMBER`. (If the environment variable is unset or empty, it will not add additional links.)

```javascript
{
  "customizations": {
    "linkOnChange": {
      "enabled": true,
      "links": [
        {
          "targetId": "{env:ENVVAR_WITH_PR_NUMBER}",
          "relationship": "GitHub Pull Request",
          "linkTemplate": "https://github.com/my-org/my-project/pull/{id}"
        }
      ]
    }
  }
}
```

