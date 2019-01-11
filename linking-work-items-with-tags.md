# Linking work items using tags

SpecSync can synchronize the scenarios with TFS test cases. In order to be able to use these test cases in TFS, you might want to link them to other work items. For example, if the scenario is describing the behavior of the user story \#123, you make a link between the test case synchronized from the scenario and the story. This gives you a better traceability and allows you to create requirement- or query-based test suites in TFS. For example you can create a test suite that contains all test cases \(scenarios\) that are related to user story \#123.

Although you can establish these links manually, once SpecSync has created the test cases from the scenarios, it is an error-prone manual process. A better way to do this is to mark the relation of the scenarios to other work items via tags and let SpecSync create the necessary links between the work items.

For that you need to mark the scenarios with tags, like `@story:123`, and specify the tag prefix \(in this case `story`\) in the SpecSync [configuration file](configuration.md). The links can be configured in the [`synchronization/links` section](configuration-synchronization-links.md) of the configuration. 

```
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

This will synchronize the scenario with the TFS test case and establish a link between the test case and the user story \#123 work item.

## Work item tags

The work item tags should follow the pattern `@prefix:N`, where `prefix` is a label of your choice \(e.g. `story`, `bug` or `wi`\) and `N` is the ID of the related work item \(e.g. `123`\).

The work item tag can be specified at scenario level or feature level. In the latter case, it behaves like you would have applied the tag for all individual scenarios. This can be useful if all scenarios in the feature file are related to a feature or a user story work item.

If the work item with the specified ID does not exist or the user who performs the synchronization does not have permission for it, SpecSync will display an error message for that particular scenario. If at least one scenario has failed to synchronize, the command line tool returns with the exit code 10.

*Note: Tags, where `N` is not a valid number, are ignored.*

## Using multiple tag prefixes

In order to distinguish between the different relations in the feature file, you can also use and synchronize multiple prefixes. E.g. you can tag a scenario with `@story:123` and `@bug:456` indicating that the scenario was related to the user story \#123 and it fixes the bug \#456.

In order to use multiple tag prefixes, you have to list multiple link type configurations within the `synchronization/links` section.

```
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

*Note: SpecSync does not check the type of the referred work item. Specifying `@bug:123` will also make the link between the user story \#123 and the test case. Though you can establish different link types for the different prefixes.*

## Link types

SpecSync creates a "Tests" link type between the test cases and the other work items by default. If you want to establish another kind of relationship \(e.g. Child\), you can specify the link type in the `relationship` setting.

```
{
  ...
  "synchronization": {
	...
    "links": [
      {
        "tagPrefix": "story",
        "relationship": "Child"
      }
    ],
	...
  },
  ...
}
```

*Note: The link type name is case sensitive and might contain spaces.*

*Note: Currently changing the link type will not trigger the re-synchronization of the scenario. If the scenario has been synchronized already, you have to to force synchronization using the [`--force` command line option](usage.md).*

## Limitations

* Existing test case links are not removed automatically, even if you remove the tag from the scenario. They have to be removed manually.
* Links are not supported for two-way synchronization.
