# Licensing

SpecSync provides different licensing options including a free usage for smaller projects. For details and pricing, please check the [available plans](http://speclink.me/specsyncplans).

Please send your questions or license inquiries to [specsync@specsolutions.eu](mailto:specsync@specsolutions.eu).

The licenses are provided as a license file. SpecSync tries to find the license file in the project folder \(where the `specsync.json` configuration file is located\). Since the license files are project or enterprise specific, these can be safely checked in to the source control.

The location of the license file \(absolute or relative to the project folder\) can also be specified with the `--license` option of the [synchronization tool](usage.md) or in the `toolSettings/licensePath` setting of the [configuration file](configuration/configuration-toolsettings.md) if necessary.

## Limitations of the Free Edition

* Maximum 30 scenarios can be synchronized. Scenario Outlines count as multiple scenarios based on the number of examples defined for the scenario outline.
* Enterprise features cannot be used

## Limitations of the Standard Edition

* Maximum 300 scenarios can be synchronized. Scenario Outlines count as multiple scenarios based on the number of examples defined for the scenario outline.
* Enterprise features cannot be used

## Enterprise features

* Two-way synchronization \(using the `pull` command\). See [Two-way synchronization](important-concepts/two-way-synchronization.md) for details.
* Branch-tag support -- supports synchronization of scenarios on a feature branch.
* Field default value support -- enables setting default values to test case fields. Useful for custom Azure DevOps process templates.
* Custom field update support -- enables updating test case fields that are normally not changed by SpecSync.
* Test case step ignore -- can ignore \(leave unchanged\) test case steps with a specific prefix.

