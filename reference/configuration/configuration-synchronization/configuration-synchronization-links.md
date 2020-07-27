# links

This configuration section contains settings to configure how test case links should be updated based on the tags of the scenario.

Read more about synchronizing test case links in the [Linking work items using tags](../../../important-concepts/linking-work-items-with-tags.md) concept description.

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

```text
{
  ...
  "synchronization": {
    ...
    "links": [
      {
        "tagPrefix": "pbi",
        "targetWorkItemType": "ProductBacklogItem",
        "relationship": "Parent",
        "mode": "createIfMissing"
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

## Settings

The `links` sub-section contains an array of link type configurations. Each of them is used to configure a link type. \(The second example above configures two link types.\)

For each link type configuration the following settings can be used.

* `tagPrefix` -- A tag prefix for specifying the relation for the scenario. E.g. specify `pbi` for linking product backlog items using a tag, like `@pbi:1234`. \(Mandatory.\)
* `targetWorkItemType` -- The type of the Azure DevOps work item the link refers to. \(Default: _\[Can link to any work item type\]_\)
* `relationship` -- Specify the relationship for the created link. E.g. specifying `Parent` means that the linked work item will be the parent of the test case work item. \(Default: `Tests` relationship is established\)
* `mode` -- Specifies how the links are updated. Available options: `createIfMissing`. \(Default: `createIfMissing`\)
  * `createIfMissing` -- SpecSync only creates links but never removes them, even if the tag has been removed from the scenario. 

_\[Back to the_ [`synchronization` _Configuration_](./)_\]_

_\[Back to the_ [_Configuration guide_](../)_\]_

