# Licensing

SpecSync provides different licensing options including a free usage for smaller projects. For details and pricing, please check the [available plans](http://speclink.me/specsyncplans).

Please send your questions or license inquiries to [specsync@specsolutions.eu](mailto:specsync@specsolutions.eu).

## License files and license keys

SpecSync licenses can be provided in two formats: as a license file or as a license key.

### License files

License files are digitally signed text files that contain license information. SpecSync tries to find the license file in the project folder (where the `specsync.json` configuration file is located). Since the license files are project or enterprise specific, these can be safely checked in to the source control.

The location of the license file (absolute or relative to the project folder) can be specified with the `--license` option of the [synchronization tool](reference/command-line-reference/) or in the `toolSettings/licensePath` setting of the [configuration file](reference/configuration/configuration-toolsettings.md) if necessary. You can also consider to configure the license path in the [user-specific configuration file](features/general-features/hierarchical-configuration-files.md#user-specific-configuration-files).

{% hint style="info" %}
The SpecSync license files are digitally signed text files. You can open them with a text editor (e.g. Notepad) to check the expiry date, the edition and other details.
{% endhint %}

### License keys

As an alternative to license files, SpecSync supports license keys. A license key is a text string that can be specified directly in the configuration file or provided via command line option or environment variable. This approach is particularly useful for secure environments such as CI/CD pipelines where you don't want to store license files.

{% hint style="warning" %}
This feature is available with SpecSync v5 or above. 
{% endhint %}

The license key can be specified in the following ways:

* In the `toolSettings/license` configuration setting of the [configuration file](reference/configuration/configuration-toolsettings.md) or the [user-specific configuration file](features/general-features/hierarchical-configuration-files.md#user-specific-configuration-files)
* With the `--license` command line option of the [synchronization tool](reference/command-line-reference/)
* Via the `SPECSYNC_LICENSE_KEY` environment variable (SpecSync automatically loads the license key from this variable)
* In the `toolSettings/license` configuration setting by loading from an custom environment variable using the `{env:ENV_VAR}` syntax

You can convert an existing license file to a license key using the `specsync upgrade` command, or you can request your license key by contacting [support](contact/specsync-support.md).

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

The Service Level Agreement \(SLA\) for the support services for SpecSync for Azure DevOps Enterprise license holders can be downloaded from [here](https://content.specsolutions.eu/specsync/SpecSync-Enterprise-Support-SLA.pdf).

## EULA

The End User License Agreement (EULA) can be downloaded [here](https://content.specsolutions.eu/specsync/EULA-SpecSync.pdf).

SpecSync for Azure DevOps uses 3rd party software libraries, please find their licenses at [http://speclink.me/specsync3rdpartylic](http://speclink.me/specsync3rdpartylic).

