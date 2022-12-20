# Excluding scenarios from synchronization

By default all scenarios in all feature file of the [local feature-set](../../important-concepts/how-to-define-the-local-feature-set-to-be-synchronized.md) are included in the synchronization, but SpecSync can be configured to exclude certain scenarios by specifying the local scope. 

{% hint style="info" %}
The local scope should be used to permanently exclude some scenarios from the synchronization \(e.g. the ones that are tagged with `@ignore`\). In order to temporarily skip scenarios for a particular synchronization command execution, you can use the filtering function instead. See [Filters and scopes](../../important-concepts/filters-and-scopes.md) for details.
{% endhint %}

## Excluding scenarios using tags

The most common way to restrict the local scope is by using scenario tags. By specifying a tag expression for the `local/tags` configuration setting, you can restrict the synchronization scope. \(See also the reference documentation of the [`local` configuration section](../../reference/configuration/configuration-local.md)\). 

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

## Excluding scenarios by specifying feature files

{% hint style="info" %}
This feature is supported in SpecSync v3.3 or later.
{% endhint %}

If the configured folder or .NET project also contains feature files that you don't want to include in synchronization, you can also restrict the local scope by specifying the set of feature files. For this you need to specify one ore more path expressions \([glob patterns](https://en.wikipedia.org/wiki/Glob_%28programming%29)\) in the `local/sourceFiles` configuration setting. \(See also the reference documentation of the [`local` configuration section](../../reference/configuration/configuration-local.md)\). 

The following example sets the local scope to the scenarios that only files from `Folder1` and files with name start with `alpha` from `Folder3` are included in the synchronization.

```text
{
  ...
  "local": {
    "sourceFiles": [
      "Folder1/",
      "Folder3/**/alpha*.feature"
    ]
  },
  ...
}
```

Besides standard glob expressions, the settings can also contain logical expressions as well, e.g. `Folder1/*.feature and not **/B.feature`. File paths containing white spaces have to be wrapped with quotes (`"`) or apostrophes (`'`), e.g. `'My Folder/A.feature'`. To see all options to express the conditions, please check [Local test case conditions](../general-features/local-test-case-conditions.md).
