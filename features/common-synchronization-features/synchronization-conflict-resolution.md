# Synchronization conflict resolution

The *push* and *pull* commands can be used to synchronize the local test cases (e.g. scenarios in a feature file) to Azure DevOps or from Azure DevOps respectively. For the most of the time this can be done without problems, because either the local test case or the remote Test Case was modified or they have only been changed in a detail that does not effect the synchronization. In some cases though a conflict can occur. This page summarizes the options to handle such conflicts.

{% hint style="info" %}
It is recommended to agree on a process about changing the tests with the team. Following a clear guideline on when to change the local test case and when the remote Azure DevOps Test Case is easier than to resolve change conflicts.
{% endhint %}

The conflict resolution method can be selected and configured separately for *push* and *pull* and by default they are set to defaults that are appropriate for the most of the usages (`overwrite` for push and `interactive` for pull).

The chosen conflict resolution method can be configured using the [`/synchronization/push/conflictResolution`](../../reference/configuration/configuration-synchronization/configuration-synchronization-push.md) and [`/synchronization/pull/conflictResolution`](../../reference/configuration/configuration-synchronization/configuration-synchronization-pull.md) settings. 

The following table contains the usable conflict resolution methods. 

| Method | Description |
| ------ | ----------- |
| `overwrite` | Overrides the target without asking. Useful if the "source-of-truth" is clearly defined. This is the default value for the *push* command. |
| `overwriteWithWarning` | Overrides the target and reports a warning. Use when you want to keep non-interactive overwrite behavior but still get notified. |
| `interactive` | In case of a conflict SpecSync shows the two versions and asks the user interactively to choose. When running on the pipeline or in a non-interactive console, the conflicts will fail the test synchronization, similarly to the `error` option. This is the default value for the *pull* command. |
| `error` | The conflicting tests will be ignored and a test error is registered. The whole synchronization process will fail, but SpecSync will try synchronizing the upcoming scenarios. |
| `skipWithWarning` | Skips the conflicting test and reports a warning (no changes made). |
| `skip` | Skips the conflicting test without a warning (no changes made). |

With the `--force` [command line option](../../reference/command-line-reference/README.md), the changes can be overridden independently of the configured method. To apply a forced override for a single test, the `--force` option can be used together with the `--filter` option, e.g. `--force --filter "@tc:1234"`.

{% hint style="info" %}
Choosing for the `error` method can be also used to get notified about conflicting changes on the server.
{% endhint %}
