# What is my TFS project URL

In order to configure the remote TFS server for synchronizing the scenarios to, the TFS project URL has to be configured in the [`remote` Configuration](configuration/configuration-remote.md).

Depending on the configuration or the installation of the TFS server, the TFS project URL might look different. 

Typical valid TFS project URLs:

* `http://myserver:8080/tfs/project-name` -- on premises TFS installation
* `https://myserver/tfs/project-name` -- on premises TFS installation with HTTPS
* `http://myserver:8080/tfs/DefaultCollection/project-name` -- on premises TFS installation with a project collection
* `http://myserver:8080/tfs/project%20name` -- space in the project name (`project name`)
* `https://myorganization.visualstudio.com/MyProject` -- Azure DevOps project created earlier
* `https://dev.azure.com/myorganization/MyProject` -- Azure DevOps project created recently

## Multi-team projects

The confusion about the TFS project URL is partly due to the fact that the TFS "project dashboard" that is the usual starting point of using TFS in the browser is not necessarily a "TFS project". Many organizations use TFS teams and multi-team TFS projects. 

TFS teams have their own dashboard and the majority of the project members (who only belong to one team within the project) use the team dashboard as a TFS project root. The team dashboard URL usually looks like this:

```
http://myserver:8080/tfs/project-name/team-name
``` 

For SpecSync, the real project URL has to be specified, so for the example above, it would be `https://myserver:8080/tfs/project-name`.

For multi-team projects the team members might not have access to all work items within the project but only under the *area path* of their team. Because of this, the newly created test cases have to be saved under this area path, otherwise SpecSync will not be able to save the test case. For this the `synchronization/areaPath` configuration setting can be used (see [Add new test cases to an Area or an Iteration](add-new-test-cases-to-an-area-or-an-iteration.md) for more details).

```
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

TFS groups the projects into project collections, but the majority of the companies use a single project collection. In earlier versions of TFS, the project collection name was mandatory in the URL, so a typical project URL looked like this:

```
http://myserver:8080/tfs/DefaultCollection/project-name
``` 

In the recent versions the default project collection name can be omitted:

```
https://myserver:8080/tfs/project-name
``` 
 
SpecSync accepts both formats.

## Spaces in project names

Some TFS project name has spaces. For the proper represenation of the TFS project URL, the spaces have to be replaced by `%20`. So the project URL of the project `SpecSync4TFS Demo` should be:

```
https://specsyncdemo.visualstudio.com/SpecSync4TFS%20Demo
```

The same representation has to be used for spaces in project collection names too.