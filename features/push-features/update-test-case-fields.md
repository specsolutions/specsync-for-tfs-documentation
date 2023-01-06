# Update Test Case fields

SpecSync updates different Test Case fields automatically based on the scenario. The other fields (either custom or built-in) can be also updated using the _Update Test Case fields_ feature.

{% hint style="info" %}
For updating custom fields or non-default Test Case fields requires an [Enterprise license](../../licensing.md). For a list of fields that can be updated with Free or Standard license, please check the [Default Test Case fields](../../reference/configuration/configuration-synchronization/configuration-synchronization-fieldupdates.md#default-test-case-fields) list in the reference.
{% endhint %}

In order to use this feature, the `synchronization/fieldUpdates` section of the configuration has to be used. The complete reference of the configuration settings can be found in the [fieldUpdates configuration reference](../../reference/configuration/configuration-synchronization/configuration-synchronization-fieldupdates.md).

## Simple field updates

The following example shows a basic configuration that initializes the _Description_ field to a message containing the feature name.

```javascript
{
  ...
 "synchronization": {
    "fieldUpdates": {
      "Description": "Synchronized from feature {feature-name}"
    }
  }
  ...
}
```

The fields can be specified with either their name or their field identifier (e.g. `System.Description`). The fields that are read-only (e.g. `Id`) or managed by SpecSync (e.g. `Tags`) cannot be updated with this setting.

{% hint style="info" %}
The field names and identifiers used in this documentation page are informational only and not reference names. They might not exist in your Azure DevOps configuration or they might have different names. For setting fields always refer to your Azure DevOps configuration for the exact field names to be used. The reference names of the built-in fields can be found in the [Azure DevOps documentation](https://docs.microsoft.com/en-us/azure/devops/boards/work-items/guidance/work-item-field?view=azure-devops). The reference name of the custom fields can be obtained from the Azure DevOps project administrator, but usually they follow the format `Custom.<your-field-name>` (e.g. `Custom.BusinessRule`).
{% endhint %}

The values can contain placeholders (e.g. `{scenario-description}`) that is replaced by SpecSync with the related value. The list of all possible placeholders can be found in the [reference](../../reference/configuration/configuration-synchronization/configuration-synchronization-fieldupdates.md#update-placeholders).

## Update fields at different update events

By default the specified field is updated "always", where always means that if the Test Case field is different from the specified value, SpecSync will update the Test Case (even if all other fields are up-to-date). The default behavior represents the classic "synchronization" scenario, where a target field needs to be kept up-to-date with the local test case settings.

It is also possible though to update the fields at certain update events, e.g. only when the new Test Case is created by SpecSync ("onCreate"). The following example initializes the *State* field to `Design` when the Test Case is created (i.e. when the scenario is first time linked).

```javascript
{
  ...
 "synchronization": {
    "fieldUpdates": {
      "State": {
        "value": "Design",
        "update": "onCreate"
      }
    }
  }
  ...
}
```

To be able to specify the update event, instead of a simple value we used an *update value specifier*. Check the [reference guide](../../reference/configuration/configuration-synchronization/configuration-synchronization-fieldupdates.md) for all possible settings of the update value specifier.

The following table shows the different supported update events.

| Update event setting | Description |
| -------------------- | ----------- |
| `always` | The target field is kept in sync with the value specifier. The Test Case is updated whenever the field value is different from the expected value. |
| `onChange` | The field value is updated only if the local test case (scenario) has changed, ie. when SpecSync would otherwise update the Test Case. If the fields managed by SpecSync are up-to-date, the field will not be updated even if the expected value is different. This setting will not update the Test Case when it is first time created. If that is also needed the `onCreateOrChange` setting has to be used. |
| `onCreate` | The field is updated (initialized) hen the new Test Case is created by SpecSync (i.e. when the scenario is first time linked) |
| `onCreateOrChange` | The field is updated either when the new Test Case is created by SpecSync or when the the local test case (scenario) has changed |

## Update fields conditionally

In some cases the field should be updated only when a certain condition is satisfied. The *update value specifier* can be configured to update the field only if a [local test case condition](../../features/general-features/local-test-case-conditions.md) evaluates to true for the scenario currently being synchronized.

The following example sets the _State_ field to `Done` once a `@done` tag is added to the scenario. When the scenario is not marked with `@done` SpecSync will not change the field value so it remains as it is manually set in Azure DevOps.

```javascript
{
  ...
 "synchronization": {
    "fieldUpdates": {
      "State": {
        "condition": "@done",
        "value": "Done"
      }
    }
  }
  ...
}
```

If you set the `condition` setting above to `not @inprogress`, the field is going to be updated if someone removes the `@inprogress` tag from the scenario.

By default, if the update event is `always` or unset, SpecSync removes the matching tag from the tag list to be synchronized. So in the example above, when the scenario is marked with `@done`, the _State_ field will be updated to `Done`, but there will be no tag (label) `done` added to the Test Case. If you would like to keep the tags in the condition even if they have been used for a field update, the `removeMatchingTags` setting has to be set to `false` as the following example shows.


```javascript
{
  ...
 "synchronization": {
    "fieldUpdates": {
      "State": {
        "condition": "@done",
        "value": "Done",
        "removeMatchingTags": false
      }
    }
  }
  ...
}
```

## Update fields based on wildcard tag match

The scenarios might be annotated with additional information using tags. E.g. the state of the priority of the scenario can be highlighted with tags like `@priority:high`, `@priority:low`. This information can be preserved in the Test Case as well with wildcard tag match.

The following example sets the _Priority_ field based on the tags defined in the format `@priority:<VALUE>`.

```javascript
{
  ...
 "synchronization": {
    "fieldUpdates": {
      "Priority": {
        "condition": "@priority:*",
        "value": "{1}"
      }
    }
  }
  ...
}
```

{% hint style="info" %}
In the examples here we used the tag format `@key:value`, but the setting can also be used to work with tags with different format, e.g. `@key=value`. By setting the condition above to `@priority=*` it would support tags, like `@priority=high`.
{% endhint %}


The `{1}` placeholder in the `value` setting refers to the value the wildcard (`*`) was matching to. So in case there was a tag `@priority:high`, the `{1}` placeholder will be replaced by `high`.

The condition can contain multiple tag wildcards as well. In that case you can use `{1}`, `{2}` etc placeholders. The following example sets the _Area_ field based on the `@service:<SERVICE-VALUE>` and `@module:<MODULE-VALUE>` tags.

```javascript
{
  ...
 "synchronization": {
    "fieldUpdates": {
      "Area": {
        "condition": "@service:* and @module:*",
        "value": "/{1}/{2}"
      }
    }
  }
  ...
}
```

## Update fields to different values using a switch

Sometimes a field can hold a value from a fixed list. The easiest if the scenarios are annotated with tags of a specific format so that the value can be obtained using wildcard tag match described above. If this is not possible (e.g. because the codebase has been annotated already with tags that does not fully match to the field values) you can use the `conditionalValue` setting to define the possible values in a switch-style. 

The following example sets the _State_ field to different values based on different tags on the scenario.


```javascript
{
  ...
 "synchronization": {
    "fieldUpdates": {
      "State": {
        "conditionalValue": {
          "@design": "Design",
          "@inprogress and not @review": "InProgress",
          "@review": "Review",
          "@done": "Done",
          "otherwise": "Unknown"
        }
      }
    }
  }
  ...
}
```

The `conditionalValue` setting contains a list of conditions that are evaluated sequentially. The value for the first matching condition will be used as field value. If none of the conditions are evaluated to true the field will not be set, except if the last condition is set to the special `otherwise` value.

The `conditionalValue` setting can also be combined with the tag wildcard match. The following example sets the _Priority_ field based on tags if they are present and sets it to `normal` if there is no priority tag on the scenario.

```javascript
{
  ...
 "synchronization": {
    "fieldUpdates": {
      "Priority": {
        "conditionalValue": {
          "@priority:*": "{1}",
          "otherwise": "normal"
        }
      }
    }
  }
  ...
}
```
