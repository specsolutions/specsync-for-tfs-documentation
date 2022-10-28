# Compatibility

## Supported Azure DevOps systems <a href="supported-server-systems" id="supported-server-systems"></a>

SpecSync for Azure DevOps might work with other Azure DevOps installations as well, but it has been tested with the following configurations:

* Azure DevOps Services (Visual Studio Team Services, VSTS, VSO)
* Azure DevOps Server 2020 (Version 18.\*)
* Azure DevOps Server 2019 (Version 17.\*, Dev17.\*)
* Team Foundation Server 2018 (Version 16.\*)
* Team Foundation Server 2017 (Version 15.\*)
* Team Foundation Server 2015 Update 3 (Version 14.\*) — _up to SpecSync v2.1_
* Team Foundation Server 2013 Update 1 (Version 12.\*) — _up to SpecSync v1.3_

{% hint style="warning" %}
The Azure DevOps team [deprecated TLS 1.0/1.1 access for Azure DevOps Services](https://devblogs.microsoft.com/devops/deprecating-weak-cryptographic-standards-tls-1-0-and-1-1-in-azure-devops-services/) (cloud version). As the Azure DevOps client library SpecSync uses for v2.1 or earlier uses TLS 1.1 by default you will get an error. To be able to use SpecSync v2.1 and earlier versions with Azure DevOps Services, you need to set the `remote/securityProtocol` setting to `Tls12`.
{% endhint %}

{% hint style="info" %}
To use SpecSync with localized Azure DevOps processes (where the names of the work item types are translated), the name of the Test Case and the Test Suite work item has to be specified in the `toolSettings/testCaseWorkItemName` and `toolSettings/testSuiteWorkItemName` configuration settings.
{% endhint %}

## Supported operating systems and platforms

SpecSync is supported on Windows, Linux and MacOS operating systems. It can be installed as a standalone tool or as a .NET tool. Please check Installation & Setup for details on which installation option is available on the different target systems.

{% hint style="info" %}
SpecSync does not use Log4J and therefore it is not affected by the vulnerability [CVE-2021-44228](https://github.com/advisories/GHSA-jfh8-c2jp-5v3q).
{% endhint %}

## Supported BDD tools

SpecSync can synchronize any scenarios that are written in Gherkin format. Gherkin format is used by many tools in many platforms, like Cucumber, Cucumber JVM, Cucumber.js, Behat, Behave and also SpecFlow. Please refer to the [Getting started](../getting-started/) for detailed instructions how to setup SpecSync with the different BDD tools.

## Supported test result formats

| BDD Tool                                                        | Runner                   | Extension | Format specifier       |
| --------------------------------------------------------------- | ------------------------ | --------- | ---------------------- |
| SpecFlow                                                        | MsTest                   | TRX       | `trx`                  |
| SpecFlow                                                        | NUnit                    | TRX       | `trx`                  |
| SpecFlow                                                        | xUnit                    | TRX       | `trx`                  |
| SpecFlow                                                        | SpecFlow+ Runner         | TRX       | `trx`                  |
| SpecFlow                                                        | NUnit                    | XML       | `specFlowNUnitXml`     |
| SpecFlow                                                        | NUnit v2                 | XML       | `specFlowNUnit2Xml`    |
| Cucumber (Java, JavaScript, Ruby)                               | `json` formatter         | JSON      | `cucumberJson`         |
| Cucumber Java                                                   | JUnit                    | XML       | `cucumberJavaJUnitXml` |
| Cucumber.js                                                     | cucumber-junit-formatter | XML       | `cucumberJsJUnitXml`   |
| [Cypress](../important-concepts/using-specsync-with-cypress.md) |                          | JSON      | `cypressCucumberJson`  |
| Jest                                                            | jest-cucumber            | XML       | `jestCucumberXml`      |
| Python Behave                                                   |                          | XML       | `pythonBehaveXml`      |
| PyTest.BDD                                                      |                          | XML       | `pyTestBddXml`         |
| Cucumber Studio (Hiptest)                                       |                          | JSON      | `hiptestJson`          |

Other test result formats are added on request. Please [contact support](../contact/specsync-support.md).

## Supported SpecFlow versions <a href="supported-specflow-versions" id="supported-specflow-versions"></a>

Synchronizing scenarios from SpecFlow projects and publishing test results is supported for **any SpecFlow versions**. 

## Supported SpecFlow versions for test-suite based execution

To be able to use the legacy [test-suite based execution](../features/test-result-publishing-features/support-for-azure-devops-test-plan-test-suite-based-test-execution.md) method, a special SpecFlow plugin has to be used that is specific to a particular SpecFlow version. This plugin is provided for the following SpecFlow versions.

* SpecFlow v3.9 (from SpecSync v3.2.6)
* SpecFlow v3.8 (from SpecSync v3.2.6)
* SpecFlow v3.7 (from SpecSync v3.2.1)
* SpecFlow v3.6 (from SpecSync v3.2)
* SpecFlow v3.5 (from SpecSync v3.2)
* SpecFlow v3.4 (from SpecSync v3.0.2)
* SpecFlow v3.3 (from SpecSync v2.1.14)
* SpecFlow v3.1 (from SpecSync v2.1.10)
* SpecFlow v3.0 (from SpecSync v2.1)
* SpecFlow v2.4 (up to SpecSync v3.2)
* SpecFlow v2.3 (up to SpecSync v3.2)
* SpecFlow v2.2.1 (with SpecSync v1.4.1)

For test-suite based execution, Azure DevOps supports only MsTest officially, but the plugins work also with MsTest V1 & V2, NUnit and xUnit on Azure DevOps and MsTest V1 on TFS2018 or earlier.

See more details on the [Support for Azure DevOps Test Plan / Test Suite based test execution](../features/test-result-publishing-features/support-for-azure-devops-test-plan-test-suite-based-test-execution.md) page.
