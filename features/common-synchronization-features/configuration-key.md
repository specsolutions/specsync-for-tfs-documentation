# Configuration key

A *configuration key* is a unique identifier for a SpecSync synchronization configuration that helps distinguish between different configurations synchronizing to the same Azure DevOps project. 

The primary purpose of the configuration key is to prevent conflicts when multiple SpecSync configurations might synchronize the same Azure DevOps Test Cases. Without a configuration key, multiple configurations could inadvertently synchronize the same Test Cases, leading to a flip-flop effect where different changes keep overwriting each other.

Configuring a configuration key is optional, but recommended, especially when using [remote scopes](remote-scope.md).

{% hint style="warning" %}
This feature is available with SpecSync v5 or above. 
{% endhint %}

## Configuring the configuration key

The configuration key is configured using the `configurationKey` configuration setting in the root of the configuration file.

The key should be a unique identifier (without spaces) within the Azure DevOps project. The maximum length is 80 characters.

The following configuration file configures a configuration key `backend-tests`.

{% code title="specsync.json" %}
```json
{
  "$schema": "https://schemas.specsolutions.eu/specsync4azuredevops-config-v5-latest.json",
  "compatibilityVersion": "5",
  "configurationKey": "backend-tests",
  "remote": {
    "projectUrl": "[...]"
  },
  ...
}
```
{% endcode %}

## How configuration key is used

The configuration key is used by SpecSync in several ways to ensure proper isolation between configurations:

### Integration with remote scopes

When using [remote scopes](remote-scope.md), the configuration key is essential for identifying which Test Cases belong to which configuration. 

For example, when using a `tag` remote scope type, the Azure DevOps Test Cases in the remote scope are marked with the tag `specsync:<config-key>`. This makes it easy to identify which configuration synchronized a particular Test Case and allows querying Test Cases in Azure DevOps by configuration.

Similarly, when using a `managedQuery` remote scope type, the configuration key is used to identify the internal Azure DevOps shared queries that store the Test Case IDs for the remote scope.

See the [Remote scope](remote-scope.md) page for more details on how configuration keys are used with remote scopes.

### Marking removed Test Cases

When SpecSync detects that a local test case has been removed (e.g., a scenario was deleted from a feature file), the corresponding Azure DevOps Test Case is tagged with `specsync:removed` and additionally with `specsync:removed-from:<config-key>` if a configuration key is specified. This helps identify which configuration was used to synchronize the Test Case and makes it easier to clean up removed Test Cases.

## Choosing a configuration key

When choosing a configuration key, consider the following guidelines:

* **Make it descriptive**: Use a key that clearly identifies the purpose or scope of the configuration, such as `backend-api-tests`, `web-ui-tests`, or `mobile-ios-tests`.
* **Keep it short**: While the maximum length is 80 characters, shorter keys are easier to work with, especially in tags and queries.
* **Use only identifier characters**: Avoid spaces and special characters. Use lowercase letters, numbers, and hyphens or underscores for word separation.
* **Ensure uniqueness**: The key must be unique within the Azure DevOps project to avoid conflicts between configurations.

## When to use a configuration key

You should configure a configuration key in the following scenarios:

* When using [remote scopes](remote-scope.md) with the `tag` or `managedQuery` remote scope types (these types require a configuration key).
* When multiple SpecSync configurations synchronize to the same Azure DevOps project, even if they synchronize different Test Cases.
* When you want better visibility and traceability of which configuration synchronized which Test Cases.

Even if you currently have only one SpecSync configuration, setting up a configuration key is recommended as a best practice to prepare for potential future configurations and to enable the full capabilities of remote scopes.
