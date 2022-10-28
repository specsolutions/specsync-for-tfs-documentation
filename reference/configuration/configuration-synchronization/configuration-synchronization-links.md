# links

This configuration section contains settings to configure how test case links should be updated based on the tags of the scenario.

Read more about synchronizing test case links in the [Linking work items using tags](../../../features/common-synchronization-features/linking-work-items-with-tags.md) concept description.

The following example shows how this sub-section can be used to specify a single link type.

```javascript
{
  ...
  "synchronization": {
    ...
    "links": [
      {
        "tagPrefix": "bug"
      }
    ],
    ...
  },
  ...
}
```

The following example shows the available options within this sub-section. This example configures two link types.

```javascript
{
  ...
  "synchronization": {
    ...
    "links": [
      {
        "tagPrefix": "pbi",
        "targetWorkItemType": "ProductBacklogItem",
        "relationship": "Parent"
      },
      {
        "tagPrefix": "bug"
      },
      {
        "tagPrefix": "pr",
        "relationship": "Pull Request"
      }
    ],
    ...
  },
  ...
}
```

## Settings

The `links` sub-section contains an array of link type configurations. Each of them is used to configure a link type. \(The second example above configures two link types.\)


For each link type configuration the following settings can be used.

| Setting | Description | Default |
| ----------------------- | ----------------------- | ----------------------- |
| `tagPrefix` | A tag prefix for specifying the relation for the scenario. E.g. specify `story` for linking product backlog items using a tag, like `@story:123`. | mandatory |
| `targetWorkItemType` | The type of the Azure DevOps work item the link refers to. | can link to any work item type |
| `relationship` | Specify the relationship for the created link. E.g. specifying `Parent` means that the linked work item will be the parent of the test case work item. Set to `Pull Request` for Azure DevOps Pull Requests and `GitHub Pull Request` for GitHub Pull Requests. | `Tests` |
| `linkTemplate` | Specifies the HTTP link template of the related artifact (for <code>GitHub Pull Request</code> relationship). The link template can use the specified value using the `{id}` placeholder. | no template used |

{% page-ref page="./" %}

{% page-ref page="../" %}

