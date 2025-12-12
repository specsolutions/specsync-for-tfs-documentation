# Customization: Synchronize linked resource titles

The _Synchronize linked resource titles_ customization can be used to synchronize linked resource (work item) titles back to the local test case tags in `@story:123;This_is_the_story_title` format. The link tags are only updated when the scenario is otherwise changed or when the `--force` option is used. 

The link prefixes this behavior should be enabled for have to be explicitly listed in the configuration.

This might be useful when you would like to improve the readability of feature files, so that you do not only see the referred resource ID, but also the descriptive title as well at the scenario tags.

{% hint style="info" %}
The _Synchronize linked resource titles_ customization described here is an [Enterprise feature](../../licensing.md).
{% endhint %}

In order to enable this customization, the `customizations/synchronizeLinkedResourceTitles` section of the configuration has to be enabled. The complete reference of the configuration settings can be found in the [customizations configuration reference](../../reference/configuration/configuration-customizations.md#synchronizelinkedresourcetitles).

The following example shows a basic configuration that synchronizes the titles of the User Story work items, liked using the tag prefix `story` (a link without title would look be `@story:123`).

If the User Story with ID 123 has the title "Allow marking pizzas as favorite", after a push synchronization is performed to a scenario linked to a User Story, the scenario tag will be updated to `@story:123;Allow_marking_pizzas_as_favorite`.

```javascript
{
  ...
  "synchronization": {
    ...
    "links": [
      {
        "tagPrefix": "story"
      }
    ]
  }
  "customizations": {
    "synchronizeLinkedResourceTitles": {
      "enabled": true,
      "linkTagPrefixes": [ "story" ]
    }
  }
  ...
}
```

The customization uses `;` by default to separate the resource title from the link, but the separator can ba changed using the `synchronization/linkLabelSeparator` setting.