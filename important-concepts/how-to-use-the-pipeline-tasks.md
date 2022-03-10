# How to use the SpecSync Azure DevOps pipeline tasks

SpecSync provides an Azure DevOps pipeline extension, called "SpecSync Tools" that contains pipeline tasks to make usage of SpecSync easier in Azure DevOps pipelines.

{% hint style="info" %}
You can find detailed information about using SpecSync on a build or release pipeline in the [How to use SpecSync from build or release pipeline](synchronizing-test-cases-from-build.md) guide.
{% endhint %}

You can find the SpecSync Tools extension in the [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=SpecSolutions.specsync-tools&targetId=3063ff79-1c38-4e95-ba9c-795b1d1eb32e).

Currently the following task is available in the extension:

* *Visual Studio Test for SpecSync* (`VsTestForSpecSync`) - a modified version of the built-in `VsTest` task that skips showing the results in the pipeline *Tests* tab to avoid duplicated display of tests.
