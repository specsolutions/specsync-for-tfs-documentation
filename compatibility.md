# Compatibility

## Supported Azure DevOps systems  <a id="supported-tfs-systems"></a>

SpecSync for Azure DevOps might work with other Azure DevOps or TFS installations as well, but it has been tested with the following configurations:

* Azure DevOps Services \(Visual Studio Team Services, VSTS, VSO\)
* Azure DevOps Server 2019
* Team Foundation Server 2018
* Team Foundation Server 2017
* Team Foundation Server 2015 Update 3 \(up to SpecSync v2.1\)
* Team Foundation Server 2013 Update 1 \(up to SpecSync v1.3\)

## Supported BDD Tools

SpecSync can synchronize any scenarios that are written in Gherkin format. Gherkin format is used by many tools in many platforms, like Cucumber, Cucumber JVM, Cucumber.js, Behat, Behave and also SpecFlow. Please refer to the [Getting started](getting-started/) for detailed instructions how to setup SpecSync with the different BDD tools.

## Supported SpecFlow versions  <a id="supported-specflow-versions"></a>

When synchronizing scenarios to non-automated test cases \(default\) or automated test cases with the "Assembly based execution" strategy, SpecSync can be used with any SpecFlow versions.

For synchronizing automated test cases with the "Suite based execution strategy", currently the following SpecFlow versions are supported.

* SpecFlow v3.3 \(from SpecSync v2.1.14\)
* SpecFlow v3.1 \(from SpecSync v2.1.10\)
* SpecFlow v3.0 \(from SpecSync v2.1\)
* SpecFlow v2.4
* SpecFlow v2.3
* SpecFlow v2.2.1 \(with SpecSync v1.4.1\)

See more details in [Synchronizing automated test cases](important-concepts/synchronizing-automated-test-cases.md).

## Supported SpecFlow unit test runners

When synchronizing scenarios to non-automated test cases \(default\) or automated test cases with the "Assembly based execution" strategy, SpecSync can be used with any test runner supported by SpecFlow.

For synchronizing automated test cases with the "Suite based execution strategy", SpecSync supports MsTest V1 & V2, NUnit and xUnit on Azure DevOps and MsTest V1 on TFS2018 or earlier.

See more details in [Synchronizing automated test cases](important-concepts/synchronizing-automated-test-cases.md).

