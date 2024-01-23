# Re-link scenarios to new Test Cases

{% hint style="info" %}
This feature is supported in SpecSync v3.3 or later.
{% endhint %}

In order to maintain multiple parallel versions of all Test Cases (e.g. one for each main release), the "Copy test plan" feature of Azure DevOps can be used in combination with the re-link command. You can read more about handling multiple parallel releases in the article [How to handle Test Cases of multiple parallel application releases](../../important-concepts/how-to-handle-test-cases-of-multiple-parallel-application-releases.md).

When the "Copy test plan" feature of Azure DevOps is used with the "Duplicate existing test cases" setting, it will create a clone of all Test Cases in the particular Test Plan. The re-link command can be used to change the Test Case link tags of the scenarios (called "re-link"), so that they refer to the cloned Test Cases.

{% hint style="info" %}
The re-link command is an [*Enterprise feature*](../../licensing.md).
{% endhint %}

When the Test Plan was cloned only once, SpecSync will able to detect the Clone Operation you have performed automatically. In case there were multiple Clone Operations, you need to specify the ID of the one you would like to process with the `--cloneOpId` option. Running the "re-link" command without specifying the Clone Operation ID will display all available Clone Operations so that you can choose the appropriate one. 

The "re-link" command will change the local workspace: it will update the Test Case link tags in the feature file and the SpecSync configuration file (e.g. `specsync.json`) according to the cloned Test Suites and Test Plans. Once the command is completed, it is recommended to review these changes and save them to source control (commit+push). 

You can find all available command line options for the re-link command in the [re-link command line reference](../../reference/command-line-reference/re-link-command.md).

In order to initialize the change tracking of the cloned Test Cases, it is also recommended to perform a push synchronization if the changes were acceptable.

{% hint style="info" %}
With SpecSync v3.3 or later all operations, including the re-link command supports "dry-run" mode using the `--dryRun` command line option. In dry-run mode, no change is made neither to Azure DevOps nor to the feature files, so you can test the impact of an operation without making an actual change.
{% endhint %}