# `synchronization/links` Configuration

This configuration section contains settings to configure how test case links should be updated based on the tags of the scenario.

Read more about synchronizing test case links in the [Linking work items using tags](../linking-work-items-with-tags.md) concept description. 

The following example shows how this sub-section can be used to specify a single link type.

```
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

```
{
  ...
  "synchronization": {
	...
    "links": [
      {
        "tagPrefix": "pbi",
        "targetWorkItemType": "ProductBacklogItem",
        "relationship": "Child",
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

The `links` sub-section contains an array of link type configurations. Each of them is used to configure a link type. (The second example above configures two link types.) 

For each link type configuration the following settings can be used. 

* `tagPrefix` -- A tag prefix for specifying the relation for the scenario. E.g. specify `pbi` for linking product backlog items using a tag, like `@pbi:1234`. (Mandatory.)
* `targetWorkItemType` -- The type of the TFS work item the link refers to. (Default: *[Can link to any work item type]*)
* `relationship` -- Specify the relationship for the created link. (Default: `Tests` relationship is established)
* `mode` -- Specifies how the links are updated. Available options: `createIfMissing`. (Default: `createIfMissing`)
  * `createIfMissing` -- SpecSync only creates links but never removes them, even if the tag has been removed from the scenario. 


*[Back to the [`synchronization` Configuration](configuration-synchronization.md)]*

*[Back to the [Configuration guide](../configuration.md)]*