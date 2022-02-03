# Release Model and Roadmap

This document contains information about the release model and the features we plan for the upcoming releases of SpecSync.

## Release model

Each SpecSync release has a unique version number that is consisting of three numbers in the *\[major\].\[minor\].\[patch\]* format, e.g. 3.2.8. The *major* and *minor* version number is generally referred as the *main* version (e.g. the main version of 3.2.8 is 3.2).

We provide three type of releases:

* **Main releases:** The main releases have a new *main* version with an increased *major* or *minor* version number. These releases might contain new features and changes that might not be backwards compatible with the previous releases. The [changelog](changelog.md) always contains detailed information about the breaking changes, but these are usually changes that affect only a smaller subset of the users or require small change in the SpecSync configuration files. If the migration requires more work, we also provide a migration guide the helps with the upgrade process. Usually there are 1 or two main releases per year. *It is recommended to upgrade to the latest *main* release at least once every year.* 

* **Update releases:** The update releases use the same *main* version number (e.g. 3.2), but have a unique *patch* number. These releases are backwards compatible with the releases of the same main version and contain bug fixes and smaller new features. The update releases are made on-demand, approximately 6 a year. We generally provide update releases for the last two main release. *It is always recommend to upgrade to the latest update release of the main version you use.* 

* **Preview releases:** Preview releases are provided on-demand for specific customers to verify a certain feature or fix before it is released as an official main or update release. The version specifier of the preview release *\[major\].\[minor\].\[patch\]-pre\[date\]-\[day-sequence\]* format and they are published to [nuget.org](https://nuget.org) as a preview release. *In general we do not recommend installing a preview release without consulting with SpecSync support.*

{% hint style="info" %}
It is always recommended to update to the latest update release as it might contain important bug fixes. It is also recommended to update to the latest main release at least once a year.
{% endhint %}

## Roadmap

### Most important features planned for v3.3

There is not yet a final date for the v3.3 release, but the final version is planned to be published in Q1 2022.

* Support for attaching files to Test Cases via tags
* Improved change detection of scenarios
* Full synchronization of Test Case links that are established through link tags (so far the links were not removed from the Test Case even if the link tag has been removed from the scenario)
* Option to generate an additional first step for scenario outlines that lists all possible parameters
* Support for updating scenario tags after performing a "Clone Test Plan" command in Azure DevOps
* Support for dry-run: synchronization attempt without making any change in Azure DevOps or in the local repository
* Support for latest Gherkin version (v20)
* Filter synchronization for feature files

### Features planned for v3.4

* Provide more options to build complex Test Suite hierarchies automatically with SpecSync.
* Interactive synchronization: ask before applying changes
