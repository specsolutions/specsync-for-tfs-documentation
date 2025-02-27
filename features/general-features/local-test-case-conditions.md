# Local test case conditions

In several configuration setting or command line option you can specify local test case conditions. For example you can specify a filter condition to only synchronize a subset of feature files.

The local test case conditions can express statements (predicates) about the tags, the source file and the name of the local test case (e.g. a scenario) and they can also contain complex logical expressions with "and", "or", "not" and grouping with parenthesis. The local test case condition syntax is based on and compatible with [Cucumber Tag expressions](https://cucumber.io/docs/cucumber/api/?lang=java#tag-expressions).

The following expression selects the scenarios that are tagged with `@important` but not tagged with `@slow`.

```
@important and not @slow
```

The use of parenthesis is shown in the example below.

```
(@important or @critical) and (not @slow)
```

The expression may contain different predicates (e.g. `@important` or `$name = 'Check password strength'`), keywords `and`, `or`, `not`, `;` and parenthesis (`(`, `)`). Whitespaces normally separate the elements, so if the predicate contains spaces, you have to wrap them to double quotes (`"@tag with space"`) or apostrophe (`'@tag with space'`). You can also mask reserved characters with `\` (`@tag_with_parenthesis\(here\)`).

The `;` is an alias for `or`, so `@bug;@feature` is equivalent to `@bug or @feature`.

The predicates in the expression can be in simple predicate form (e.g. `@important`) or as a relation (e.g. `$tags ~ @important`). Relation predicate are built up from a field name that start with `$` (e.g. `$name`), and operator (e.g. `=` or `~`) and a value (`'Check password strength'`).

The following example combines a simple tag predicate and a source file relation predicate to select all scenarios that are tagged with `@important` and are in the `pricing` folder.

```
@important and $sourceFile ~ pricing/
```

{% hint style="info" %}
When specifying conditions from the command line using the relation from, please make sure that the `$` sign is not resolved by the shell. For example in PowerShell, you have to wrap the argument with `'` to pass the expression to SpecSync literally (e.g. `--filter '$tags ~ @important'`) or mask it with backtick (e.g. `--filter "``$tags ~ @important"`).
{% endhint %}


In the following sections you can find details about how to express tag and source file predicates.

## Tag predicates

The tag predicate must start with an `@` followed by the tag name or tail wildcard tag name match. Alternatively you can use the relation form using `$tags` as field name and the `~` ("matches") operator.

{% hint style="info" %}
Before v3.4, the tag predicates were accepted without the leading `@` sign of the tag name, but in recent versions the `@` has to be used, even if the local test case does not use this prefix for tags.
{% endhint %}

When evaluating tag predicates both direct and indirect tags are considered. For example a scenario is selected not only if the scenario itself is marked with the expected tag but also if the containing rule or the feature file has the tag. Scenario outlines are also selected if there is at least one scenario outline examples block that satisfies the condition.

The following table shows the different tag predicate options.

| Name | Examples | Description |
| ---- | -------- | ----------- |
| Tag name | `@important` | Selects local test cases that directly or indirectly contain a tag with the specified name. E.g. a scenario is selected if the scenario, the related rule or the feature has the tag with the exact same name. The tag names are case sensitive! |
| Tail wildcard tag name match | `@auth*`, `@priority:*` | Selects the local test cases that directly or indirectly contain a tag that starts with the name specified before the `*`. E.g. `@auth*` matches to `@authenticated` or `@authorized`, `@priority:*` matches to `@priority:high` or `@priority:low`. The wildcard can only stand at the end of the predicate. |
| Relation form | `$tags ~ @important`, `$tags ~ @auth*` | The relation form can be used to match tags either with tag name or with tail wildcard tag name match. |

{% hint style="info" %}
In `local/sourceFiles` and `--sourceFileFilter` the simple tag predicates cannot be used. The relation form works though.
{% endhint %}

## Local test case name predicates

The local test case name predicate (or "name predicate" in short) can select the local test cases by name equality. For scenarios the local test case name is the scenario name, so with this predicates you can easily filter for specific scenarios. To filter for a condition that includes a name predicate, you can use the `--filter` command line option.

{% hint style="info" %}
The local test case name predicates have been introduced in v3.4.2.
{% endhint %}

The name predicate has to be used with the relation form using `$name` as field name and the `=` ("equals") operator. The following expression selects the scenario named `Check password strength`.

```
$name = 'Check password strength'
```

For most of the test frameworks the local test case name is not unique within the project, so filtering for name equality might process more than one tests. For more precise selection of a single test you can combine it with the source file predicates as the following example shows.

```
$sourceFile ~ **/Registration.feature and $name = 'Check password strength'
```

## Source file predicates

The source file predicates can select the local test cases using [glob patterns](https://speclink.me/specsync-glob) that are evaluated against the source file relative path (for scenarios, this is the feature file path). E.g. `Folder1/` selects any local test cases with source file within the folder `Folder1` or its sub-folders; `Folder3/**/alpha*.feature` selects any feature files within `Folder3` that starts with `alpha`.

For most expressions the source file predicate has to be used with the relation form using `$sourceFile` as field name and the `~` ("matches") operator. The following expression selects the feature files from the `pricing` folder.

```
$sourceFile ~ pricing/
```

The following example selects the files from the `pricing` folder except if their name starts with `smoke`.

```
$sourceFile ~ pricing/ and not $sourceFile ~ **/smoke*.feature
```

For `local/sourceFiles` and `--sourceFileFilter` you can use source file predicate in the simple form, e.g.

```
dotnet specsync push --sourceFileFilter '**/smoke*.feature'
```
