# Compatibility

## Supported Azure DevOps systems  <a id="supported-tfs-systems"></a>

SpecSync for Azure DevOps might work with other Azure DevOps or TFS installations as well, but it has been tested with the following configurations:

* Azure DevOps Services \(Visual Studio Team Services, VSTS, VSO\)
* Azure DevOps Server 2019
* Team Foundation Server 2017
* Team Foundation Server 2015 Update 3
* Team Foundation Server 2013 Update 1 \(up to SpecSync v1.3\)

## Supported BDD Tools

SpecSync can synchronize any scenarios that are written in Gherkin format. Gherkin format is used by many tools in many platforms, like Cucumber, Cucumber JVM, Cucumber.js, Behat, Behave and also SpecFlow. Please refer to the [Getting started](getting-started/) for detailed instructions how to setup SpecSync with the different BDD tools.

## Supported SpecFlow versions  <a id="supported-specflow-versions"></a>

When synchronizing scenarios to non-automated test cases \(default\), SpecSync can be used with any SpecFlow versions.

For synchronizing automated test cases, currently the following SpecFlow versions are supported.

* SpecFlow v3.0 \(with SpecSync v2.1.0 prerelease\)
* SpecFlow v2.4
* SpecFlow v2.3
* SpecFlow v2.2.1 \(with SpecSync v1.4.1\)

See more details in [Synchronizing automated test cases](important-concepts/synchronizing-automated-test-cases.md).

If you need to use SpecSync \(for synchronizing automated test cases\) with a SpecFlow version not listed here, please contact support \(specsync@specsolutions.eu\).

## Supported SpecFlow unit test runners

When synchronizing scenarios to non-automated test cases \(default\), SpecSync can be used with any test runner supported by SpecFlow.

_**Update \(31st Jan 2019\): The automated test case execution support has been improved in Azure DevOps therefore the limitation of only supporting MsTest v1 has been removed! In SpecSync currently this new feature can be used with our new**_ [_**SpecFlow v3 plugin**_](https://www.nuget.org/packages/SpecSync.AzureDevOps.SpecFlow.3-0)_**. We are going to update the other SpecFlow plugins as well and also update the documentation soon.**_

For synchronizing automated test cases, SpecFlow has to be used with MsTest V1. This is a limitation of the Azure DevOps test case execution infrastructure that SpecSync cannot influence.

See more details in [Synchronizing automated test cases](important-concepts/synchronizing-automated-test-cases.md).

