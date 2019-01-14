# What is my Azure DevOps project URL

In order to configure the remote Azure DevOps server for synchronizing the scenarios to, the Azure DevOps project URL has to be configured in the [`remote` Configuration](../configuration/configuration-remote.md).

Depending on the configuration or the installation of the Azure DevOps server, the Azure DevOps project URL might look different.

Typical valid Azure DevOps project URLs:

* `http://myserver:8080/tfs/project-name` -- on premises Azure DevOps installation \(Azure DevOps Server / Team Foundation Server\)
* `https://myserver/tfs/project-name` -- on premises Azure DevOps installation with HTTPS
* `http://myserver:8080/tfs/DefaultCollection/project-name` -- on premises Azure DevOps installation with a project collection
* `http://myserver:8080/tfs/project%20name` -- space in the project name \(`project name`\)
* `https://myorganization.visualstudio.com/MyProject` -- Azure DevOps project created earlier
* `https://dev.azure.com/myorganization/MyProject` -- Azure DevOps project created recently

## Multi-team projects

The confusion about the Azure DevOps project URL is partly due to the fact that the Azure DevOps "project dashboard" that is the usual starting point of using Azure DevOps in the browser is not necessarily a "Azure DevOps project". Many organizations use Azure DevOps teams and multi-team Azure DevOps projects.

Azure DevOps teams have their own dashboard and the majority of the project members \(who only belong to one team within the project\) use the team dashboard as a Azure DevOps project root. The team dashboard URL usually looks like this:

```text
http://myserver:8080/tfs/project-name/team-name
```

For SpecSync, the real project URL has to be specified, so for the example above, it would be `https://myserver:8080/tfs/project-name`.

For multi-team projects the team members might not have access to all work items within the project but only under the _area path_ of their team. Because of this, the newly created test cases have to be saved under this area path, otherwise SpecSync will not be able to save the test case. For this the `synchronization/areaPath` configuration setting can be used \(see [Add new test cases to an Area or an Iteration](add-new-test-cases-to-an-area-or-an-iteration.md) for more details\).

```text
{
  ...
  "synchronization": {
    ...
    "areaPath": {
      "mode": "setOnLink",
      "value": "\\team-name"
    },
    ...
  },
  ...
}
```

## Project collections

Azure DevOps groups the projects into project collections, but the majority of the companies use a single project collection. In earlier versions of Azure DevOps, the project collection name was mandatory in the URL, so a typical project URL looked like this:

```text
http://myserver:8080/tfs/DefaultCollection/project-name
```

In the recent versions the default project collection name can be omitted:

```text
https://myserver:8080/tfs/project-name
```

SpecSync accepts both formats.

## Spaces in project names

Some Azure DevOps project name has spaces. For the proper represenation of the Azure DevOps project URL, the spaces have to be replaced by `%20`. So the project URL of the project `SpecSync4AzureDevOps Demo` should be:

```text
https://specsyncdemo.visualstudio.com/SpecSync4AzureDevOps%20Demo
```

The same representation has to be used for spaces in project collection names too.

