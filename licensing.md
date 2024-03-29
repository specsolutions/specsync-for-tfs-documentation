# Licensing

SpecSync provides different licensing options including a free usage for smaller projects. For details and pricing, please check the [available plans](http://speclink.me/specsyncplans).

Please send your questions or license inquiries to [specsync@specsolutions.eu](mailto:specsync@specsolutions.eu).

The licenses are provided as a license file. SpecSync tries to find the license file in the project folder \(where the `specsync.json` configuration file is located\). Since the license files are project or enterprise specific, these can be safely checked in to the source control.

The location of the license file \(absolute or relative to the project folder\) can also be specified with the `--license` option of the [synchronization tool](reference/command-line-reference/) or in the `toolSettings/licensePath` setting of the [configuration file](reference/configuration/configuration-toolsettings.md) if necessary. You can also consider to configure the license path in the [user-specific configuration file](features/general-features/hierarchical-configuration-files.md#user-specific-configuration-files).

{% hint style="info" %}
The SpecSync license files are digitally signed text files. You can open them with a text editor \(e.g. Notepad\) to check the expiry date, the edition and other details.
{% endhint %}

## Limitations of the Free Edition

* Maximum 30 scenarios can be synchronized
* Enterprise features cannot be used

{% hint style="info" %}
Scenario Outlines count as one scenario.
{% endhint %}

## Limitations of the Standard Edition

* Maximum 300 scenarios can be synchronized
* Enterprise features cannot be used

{% hint style="info" %}
Scenario Outlines count as one scenario.
{% endhint %}

## Enterprise features

* Two-way synchronization \(using the *pull* command\). See [Two-way synchronization](features/pull-features/two-way-synchronization.md) for details.
* Re-linking scenarios to new Test Cases \(using the *re-link* command\). See [Re-link scenarios to new Test Cases](features/common-synchronization-features/re-link-scenarios.md) 
* All [customization features](features/customizations.md)

## Enterprise support

The Service Level Agreement \(SLA\) for the support services for SpecSync for Azure DevOps Enterprise license holders can be downloaded from [here](https://www.specsolutions.eu/media/specsync/SpecSync-Enterprise-Support-SLA.pdf).

## EULA

The End User License Agreement (EULA) can be downloaded [here](https://www.specsolutions.eu/media/specsync/EULA-SpecSync.pdf).

SpecSync for Azure DevOps uses 3rd party software libraries, please find their licenses at [http://speclink.me/specsync3rdpartylic](http://speclink.me/specsync3rdpartylic).

