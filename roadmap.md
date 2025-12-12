# Release Model and Roadmap

This document contains information about the release model and the features we plan for the upcoming releases of SpecSync.

## Release model

SpecSync uses [semantic versioning](https://semver.org/). Each SpecSync release has a unique version number that is consisting of three numbers in the *\[major\].\[minor\].\[patch\]* format, e.g. 5.2.0. The *major* version number is generally referred as the *main* version (e.g. the main version of 5.2.0 is v5).

We do not provide public patch releases (when only the *patch* number changes), the *patch* number is used as an internal identifier.

{% hint style="hint" %}
Before v5 the *major* and the *minor* version number represented the *main* version (e.g. v3.4) and the *patch* number was used for *update releases*.
{% endhint %}

We provide three type of releases:

* **Main releases:** The main releases have a new *main* version with an increased *major* version number. These releases might contain new features and changes that might not be backwards compatible with the previous releases. The [changelog](changelog.md) always contains detailed information about the breaking changes. The `upgrade` command can help to perform the majority of the upgrade steps. The breaking changes are usually changes that affect only a smaller subset of the users or require small change in the SpecSync configuration files. If the migration requires more work, we also provide a migration guide the helps with the upgrade process. Usually there are 1 or two main releases per year. *It is recommended to upgrade to the latest *main* release at least once every year.* 

* **Update releases:** The update releases use the same *major* version number (e.g. v5), but have a unique *minor* number (e.g. 5.1, 5.2). These releases are backwards compatible with the releases of the same main version and contain bug fixes and smaller new features. The update releases are made on-demand, approximately 6 a year. We generally provide update releases for the last two main release. *It is always recommend to upgrade to the latest update release of the major version you use.* 

* **Preview releases:** Preview releases are provided on-demand for specific customers to verify a certain feature or fix before it is released as an official main or update release. The version specifier of the preview release *\[major\].\[minor\].\[patch\]-pre\[date\]-\[day-sequence\]* format and they are published to [nuget.org](https://nuget.org) as a preview release. *In general we do not recommend installing a preview release without consulting with SpecSync support.*

{% hint style="info" %}
It is always recommended to update to the latest update release as it might contain important bug fixes. It is also recommended to update to the latest main release at least once a year.
{% endhint %}

{% hint style="info" %}
The [How to upgrade to a newer version of SpecSync](important-concepts/how-to-upgrade-specsync.md) guide contains information about how to upgrade to a newer version.
{% endhint %}

### SpecSync plugin compatibility

The [SpecSync plugins](features/plugin-list.md) are generally compatible with the SpecSync version with the same *major* version number. E.g. a plugin with version 5.4.3 is compatible with any v5 SpecSync release.

## Roadmap

We have just finished the v5 release. Please check the [Changelog](changelog.md#v5) or try out the new version and provide feedback!

We will update the roadmap after v5 has been released. Please stay tuned.

