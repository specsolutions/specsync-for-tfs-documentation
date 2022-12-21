# fieldUpdates

This configuration section specifies how the fields of the Test Case that are not managed by SpecSync should be updated.

Read more about updating Test Case fields in the [Update Test Case fields](../../../features/push-features/update-test-case-fields.md) feature description.

{% hint style="info" %}
For updating custom fields or non-default Test Case fields requires an [Enterprise license](../../licensing.md). For a list of fields that can be updated with Free or Standard license, please check the [Default Test Case fields](#default-test-case-fields) list below.
{% endhint %}


The following example shows how this sub-section can be used to specify update different fields.

```javascript
{
  ...
  "synchronization": {
    ...
    "fieldUpdates": {
      "field1": "value1",
      "field2": {
        "condition": "@tag1",
        "value": "value2",
        "removeMatchingTags": false
      },
      "field2": {
        "conditionalValue": {
          "@tag1": "value3",
          "@tag2": "value4",
          "otherwise": "value5"
        },
        "update": "onCreate"
      },
      "field3": {
        "condition": "@mytag:*",
        "value": "{1}"
      }      
    },
    ...
  },
  ...
}
```

## Settings

The `fieldUpdates` sub-section contains an object that is used as a dictionary of `"key": <VALUE>` pairs, where `key` is a field name or identifier and `<VALUE>` is the value to update to or an *update value specifier*. The update value specifier can be either a simple value (e.g. `"value1"`) or a value value specifier object with different settings described below.

The values (either as simple value or inside the update value specifier) can contain placeholders (e.g. `{scenario-description}`) that is replaced by SpecSync with the related value. The list of all possible placeholders can be found in the section [below](#update-placeholders).

{% hint style="info" %}
The fields can be specified with either their name or their field identifier.

The fields that are read-only (e.g. `Id`) or managed by SpecSync (e.g. `Tags`) cannot be updated with this setting.
{% endhint %}

An *update value specifier* can contain the following settings.

| Setting | Description | Default |
| ----------------------- | ----------------------- | ----------------------- |
| `update` | Specifies the event that should trigger the update. The value `always` is the default and means that the field is always kept in sync with the local test case details. Possible values: `always`, `onChange`, `onCreate`, `onCreateOrChange` | `always` |
| `condition` | A [local test case condition](../../../features/general-features/local-test-case-conditions.md) of scenarios that should be that should be considered for the update (e.g. `@inprogress`, `not @ready`). | all scenarios considered |
| `value` | The value of the field. Can contain placeholders, like `{scenario-name}` or `{1}`. The latter inserts the tag name wildcard match from condition |  |
| `conditionalValue` | A *switch*-like key-value pair of tag expressions to field values. A special key `otherwise` can be used as last to match all other cases. | |
| `removeMatchingTags` | Controls whether the tag(s) that matches to the condition in update=always field settings should be removed from the Test Case as tags. | `true`, unless the `update` is not `always` |

## Update placeholders <a href="update-placeholders" id="update-placeholders"></a>

| Placeholder                       | Description                                                                                     |
| --------------------------------- | ----------------------------------------------------------------------------------------------- |
| `{feature-name}`                  | the name of the feature (specified in the feature file header)                                  |
| `{feature-description}`           | the description of the feature (the free-text block specified after the feature file header)    |
| `{feature-source}`                | The full feature file source text.                                                              |
| `{rule-name}`                     | The name of the rule that the scenario belongs to.                                              |
| `{scenario-name}`                 | the name of the scenario or scenario outline                                                    |
| `{scenario-description}`          | the description of the scenario or scenario outline                                             |
| `{scenario-source}`               | The full scenario source text.                                                                  |
| `{feature-file-name}`             | the file name of the feature file (without folder)                                              |
| `{feature-file-folder}`           | the folder of the feature file, relative to the project root                                    |
| `{feature-file-path}`             | the path (folder and file name) of the feature file, relative to the project root               |
| `{br}`                            | a new line                                                                                      |
| `{env:ENVIRONMENT_VARIABLE_NAME}` | The content of the environment variable specified (`ENVIRONMENT_VARIABLE_NAME` in this example) |
| `{1}`                             | Refers to the wildcard (`*`) match of the condition. In case there are multiple wildcards, the `{1}`, `{2}`, etc. placeholders can be used to refer to the matches |

For the placeholders, different "value loaders" can be specified. Value loaders can transform the value. E.g. if the `HTML` loader is used in a field update as `{scenario-description:HTML}`, it will replace the white space and new line characters of the scenario description with the necessary HTML elements. 

The following value loaders are supported:

  * `HTML` - encodes HTML tags and transforms whitespaces to HTML newlines and non-breaking spaces.
  * `HtmlEncode` - encodes HTML tags.
  * `Unix` - replaces Windows-style path separators (`\`) with Unix-style ones (`/`)
  * `Windows` - replaces Unix-style path separators (`/`) with Windows-style ones (`\`)

## Default Test Case fields <a href="default-test-case-fields" id="default-test-case-fields"></a>

The following Test Case fields can be updated with Free or Standard license. For updating other fields an [Enterprise license](../../licensing.md) is required.

| TCM solution | Default Test Case fields |
| ------------ | ------------------------------- |
| Jira TCM Customization | `priority`, `status`, `resolution`, `assignee`, `reporter`, `description`, `components`, `issuekey`, `Issue`, `comment`, `Epic Link`, `Sprint` |
| Zephyr Scale | `Precondition`, `Objective`, `Folder`, `Status`, `Priority`, `Component`, `Owner`, `EstimatedTime` |

## Examples

See further examples for using the `fieldUpdates` setting in the [Update Test Case fields](../../../features/push-features/update-test-case-fields.md) feature description.

{% page-ref page="./" %}

{% page-ref page="../" %}

