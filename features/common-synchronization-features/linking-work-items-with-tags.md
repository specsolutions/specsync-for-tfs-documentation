# Linking Work Items using tags

SpecSync can synchronize the scenarios with Azure DevOps test cases. In order to be able to use these test cases in Azure DevOps, you might want to link them to other work items. For example, if the scenario is describing the behavior of the user story \#123, you make a link between the test case synchronized from the scenario and the story. This gives you a better traceability and allows you to create requirement- or query-based test suites in Azure DevOps. For example you can create a test suite that contains all test cases \(scenarios\) that are related to user story \#123.

Although you can establish these links manually, once SpecSync has created the test cases from the scenarios, it is an error-prone manual process. A better way to do this is to mark the relation of the scenarios to other work items via tags and let SpecSync create the necessary links between the work items.

For that you need to mark the scenarios with tags, like `@story:123`, and specify the tag prefix \(in this case `story`\) in the SpecSync [configuration file](../../reference/configuration/). The links can be configured in the [`synchronization/links` section](../../reference/configuration/configuration-synchronization/configuration-synchronization-links.md) of the configuration.

```text
{
  ...
  "synchronization": {
    ...
    "links": [
      {
        "tagPrefix": "story"
      }
    ],
    ...
  },
  ...
}
```

This will synchronize the scenario with the Azure DevOps test case and establish a link between the test case and the user story \#123 work item.

## Work item tags

The work item tags should follow the pattern `@prefix:N`, where `prefix` is a label of your choice \(e.g. `story`, `bug` or `wi`\) and `N` is the ID of the related work item \(e.g. `123`\).

The work item tag can be specified at scenario level or feature level. In the latter case, it behaves like you would have applied the tag for all individual scenarios. This can be useful if all scenarios in the feature file are related to a feature or a user story work item.

If the work item with the specified ID does not exist or the user who performs the synchronization does not have permission for it, SpecSync will display an error message for that particular scenario. If at least one scenario has failed to synchronize, the command line tool returns with the exit code 10.

_Note: Tags, where_ `N` _is not a valid number, are ignored._

## Using multiple tag prefixes

In order to distinguish between the different relations in the feature file, you can also use and synchronize multiple prefixes. E.g. you can tag a scenario with `@story:123` and `@bug:456` indicating that the scenario was related to the user story \#123 and it fixes the bug \#456.

In order to use multiple tag prefixes, you have to list multiple link type configurations within the `synchronization/links` section.

```text
{
  ...
  "synchronization": {
    ...
    "links": [
      {
        "tagPrefix": "story"
      },
      {
        "tagPrefix": "bug"
      }
    ],
    ...
  },
  ...
}
```

_Note: SpecSync does not check the type of the referred work item. Specifying_ `@bug:123` _will also make the link between the user story \#123 and the test case. Though you can establish different link types for the different prefixes._

## Link types

SpecSync creates a "Tests" link type between the test cases and the other work items by default. If you want to establish another kind of relationship \(e.g. Parent\), you can specify the link type in the `relationship` setting. The specified relationship defines how the linked work item is related to the test case, e.g. specifying `Parent` means that the linked work item will be the parent of the test case work item.

```text
{
  ...
  "synchronization": {
    ...
    "links": [
      {
        "tagPrefix": "story",
        "relationship": "Parent"
      }
    ],
    ...
  },
  ...
}
```

{% hint style="info" %}
The link type name is case sensitive and might contain spaces. 

For linking Pull Requests, the `relationship` setting is mandatory and has to be set to `Pull Request`.
{% endhint %}

{% hint style="warning" %}
Currently changing the link type will not trigger the re-synchronization of the scenario. If the scenario has been synchronized already, you have to to force synchronization using the [`--force` command line option](../../reference/command-line-reference/).
{% endhint %}

## Linking Pull Requests

From SpecSync v3.2 the Test Cases can also be linked to Pull Requests using tags. For that the `relationship` setting of the link specification must be set to `Pull Request`.

## Limitations

* Existing test case links are not removed automatically, even if you remove the tag from the scenario. They have to be removed manually.
* Link tags are not created when the Test Case changes retrieved with the pull command.

