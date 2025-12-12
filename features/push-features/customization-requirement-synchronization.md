# Customization: Requirement Synchronization

The _Requirement Synchronization_ customization turns selected source documents (for example `*.requirement.feature` files) into requirement work items in Azure DevOps and keeps them in sync. It can also auto-link scenarios in the same file or folder to the synchronized requirement.

{% hint style="info" %}
The _Requirement Synchronization_ customization described here is an [Enterprise feature](../../licensing.md).
{% endhint %}

Enable the customization in the `customizations/requirementSynchronization` section. The complete list of settings is available in the [customizations configuration reference](../../reference/configuration/configuration-customizations.md#requirementsynchronization).

The example below creates User Story work items from files ending with `.requirement.feature`, stores their IDs with the `@req-id:` tag prefix, links scenarios from the same file and folder, and maps requirement description and acceptance criteria to custom fields.

```javascript
{
  ...
  "customizations": {
    "requirementSynchronization": {
      "enabled": true,
      "requirements": [
        {
          "targetType": "User Story",
          "condition": "$sourceFile ~ **/*.requirement.feature",
          "tagPrefix": "req-id",
          "linkContainerTests": true,
          "linkFolderTests": true,
          "acceptanceCriteriaSeparator": "Acceptance Criteria:",
          "fieldUpdates": {
            "Custom.Description": "{requirement-description:MarkdownToHtml}",
            "Custom.AcceptanceCriteria": "{requirement-acceptance-criteria:MarkdownToHtml}"
          }
        }
      ]
    }
  }
  ...
}
```

## Linking scenarios to the requirement

- Use `linkContainerTests: true` to automatically link scenarios in the same source document to the synchronized requirement work item.
- Use `linkFolderTests: true` to also link scenarios located in the same folder (and sub-folders) as the requirement document.
- You can still use `synchronization/links` tags (e.g. `@req-tests:123`) to link scenarios to synchronized requirements (just like linking to any work item).

## Requirement source and parsing

- `source` selects whether the requirement is built from a `Feature` (default) or from a `Rule` within a feature file, allowing multiple requirements per file.
- `acceptanceCriteriaSeparator` lets SpecSync split the description into a requirement description and acceptance criteria section, which can be mapped with `fieldUpdates` (see example above).

## Branch tagging and extra links

- Use `links` to control how tags on the requirement document (for example `@bug:123`) create links from the requirement work item to other work items, and override link relationships when needed.
- When branch tagging is enabled, `branchTagPrefix` specifies how branched requirements are tagged (pairs with `customizations/branchTag`). See [Branch tag customization](customization-support-synchronizing-scenarios-from-a-branch.md) for details.
- Use `linkOnChange` to automatically link changed requirements to another work item or pull request, similarly to the [Link on change customization](customization-automatically-link-changed-test-cases.md).

Example: linking requirements to Bugs using a custom relationship

```javascript
{
  "customizations": {
    "requirementSynchronization": {
      "enabled": true,
      "requirements": [
        {
          "targetType": "User Story",
          "condition": "$sourceFile ~ **/*.requirement.feature",
          "tagPrefix": "req-id",
          "links": [
            { "tagPrefix": "bug", "relationship": "related" }
          ]
        }
      ]
    }
  }
}
```

Add a tag like `@bug:1234` to the requirement document to create the link using the specified relationship.

## Updating requirement fields

The `fieldUpdates` setting works the same way as for Test Cases and can be used to fill requirement fields from the requirement description, acceptance criteria, rules, or other placeholders. See the [configuration reference](../../reference/configuration/configuration-synchronization/configuration-synchronization-fieldupdates.md#update-placeholders) for the placeholder list and update options.
