# Customization: Synchronize linked artifact titles

The _Synchronize linked artifact titles_ customization can be used to synchronize linked artifact (Work Item) titles back to the local test case tags in `@story:1234:This_is_the_story_title` format. The link tags are only updated when the scenario is otherwise changed or when the `--force` option is used. 

The link prefixes this behavior should be enabled for have to be explicitly listed in the configuration.

This might be useful when you would like to improve the readability of feature files, so that you do not only see the referred artifact ID, but also the descriptive title as well at the scenario tags.

{% hint style="info" %}
The _Synchronize linked artifact titles_ customization described here is an [Enterprise feature](../../licensing.md).
{% endhint %}

In order to enable this customization, the `customizations/synchronizeLinkedArtifactTitles` section of the configuration has to be enabled. The complete reference of the configuration settings can be found in the [customizations configuration reference](../../reference/configuration/configuration-customizations.md#ignoretestcasetags).

The following example shows a basic configuration that synchronizes the titles of the User Story work items, liked using the tag prefix `story` (a link without title would look be `@story:42`).

If the User Story with ID 42 has the title "Allow marking pizzas as favorite", after a push synchronization is performed to a scenario linked to a User Story, the scenario tag will be updated to `@story:42:Allow_marking_pizzas_as_favorite`.

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
    "synchronizeLinkedArtifactTitles": {
      "enabled": true,
      "linkTagPrefixes": [ "story" ]
    }
  }
  ...
}
```
