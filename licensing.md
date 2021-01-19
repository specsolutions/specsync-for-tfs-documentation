# Licensing

SpecSync provides different licensing options including a free usage for smaller projects. For details and pricing, please check the [available plans](http://speclink.me/specsyncplans).

Please send your questions or license inquiries to [specsync@specsolutions.eu](mailto:specsync@specsolutions.eu).

The licenses are provided as a license file. SpecSync tries to find the license file in the project folder \(where the `specsync.json` configuration file is located\). Since the license files are project or enterprise specific, these can be safely checked in to the source control.

The location of the license file \(absolute or relative to the project folder\) can also be specified with the `--license` option of the [synchronization tool](reference/command-line-reference/) or in the `toolSettings/licensePath` setting of the [configuration file](reference/configuration/configuration-toolsettings.md) if necessary.

{% hint style="info" %}
The SpecSync license files are digitally signed text files. You can open them with a text editor \(e.g. Notepad\) to check the expiry date, the edition and other details.
{% endhint %}

## Limitations of the Free Edition

* Maximum 30 scenarios can be synchronized
* Enterprise features cannot be used

{% hint style="info" %}
From v2.1.14, Scenario Outlines count as one scenario.
{% endhint %}

## Limitations of the Standard Edition

* Maximum 300 scenarios can be synchronized
* Enterprise features cannot be used

{% hint style="info" %}
From v2.1.14, Scenario Outlines count as one scenario.
{% endhint %}

## Enterprise features

* Two-way synchronization \(using the `pull` command\). See [Two-way synchronization](features/pull-features/two-way-synchronization.md) for details.
* Branch-tag support -- supports synchronization of scenarios on a feature branch. See [Support synchronizing scenarios from a branch](features/push-features/support-synchronizing-scenarios-from-a-branch.md) for details.
* [Field default value support](features/push-features/customization-setting-test-case-fields-with-default-values.md) -- enables setting default values to test case fields. Useful for custom Azure DevOps process templates.
* [Custom field update support](features/push-features/customization-update-custom-test-case-fields-on-push.md) -- enables updating test case fields that are normally not changed by SpecSync.
* [Test case step ignore](features/push-features/customization-ignoring-marked-test-case-steps.md) -- can ignore \(leave unchanged\) test case steps with a specific prefix.
* [Mapping tags](features/push-features/customization-mapping-tags.md) -- can replace characters or sub-strings in tags
* [Publishing test results to multiple Test Suites](features/test-result-publishing-features/customization-publishing-test-results-to-multiple-test-suite.md) -- Make the test result visible in multiple Test Suites with a single test result publish command
* Integrations with 3rd party commercial products, like SpecFlow+

## Enterprise support

The Service Level Agreement \(SLA\) for the support services for SpecSync for Azure DevOps Enterprise license holders can be downloaded from [here](https://www.specsolutions.eu/media/specsync/SpecSync-Enterprise-Support-SLA.pdf).

## EULA

The End User License Agreement can be downloaded [here](https://www.specsolutions.eu/media/specsync/EULA-SpecSync.pdf).

SpecSync for Azure DevOps uses 3rd party software libraries, please find their licenses at [http://speclink.me/specsync3rdpartylic](http://speclink.me/specsync3rdpartylic).

