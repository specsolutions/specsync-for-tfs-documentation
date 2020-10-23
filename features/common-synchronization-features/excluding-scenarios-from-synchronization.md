# Excluding scenarios from synchronization

By default all scenarios in all feature file of the [local feature-set](../../important-concepts/how-to-define-the-local-feature-set-to-be-synchronized.md) are included in the synchronization, but SpecSync can be configured to exclude certain scenarios by specifying the local scope. 

{% hint style="info" %}
The local scope should be used to permanently exclude some scenarios from the synchronization \(e.g. the ones that are tagged with `@ignore`\). In order to temporarily skip scenarios for a particular synchronization command execution, you can use the filtering function instead. See [Filters and scopes](../../important-concepts/filters-and-scopes.md) for details.
{% endhint %}

Currently the local scope can be restricted using scenario tags. By specifying a tag expression for the `local/tags` configuration setting, you can restrict the synchronization scope. \(See also the reference documentation of the [`local` configuration section](../../reference/configuration/configuration-local.md)\). 

The following example sets the local scope to the scenarios that are marked as `@done` but exclude the ones that are tagged with `@ignored` or `@planned`.

```text
{
  ...
  "local": {
    "tags": "@done and not (@ignored or @planned)"
  },
  ...
}
```

