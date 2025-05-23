# Introduction to SpecSync for Azure DevOps

Many teams being currently in a transformation process towards using agile and BDD have to face the problem that the new specification artifacts and reports have to be integrated into the existing tool chain. Many of such teams use Azure DevOps-full where the concept of Test Cases and Test Runs can be used to represent and trace the testing activities. 

_SpecSync for Azure DevOps_ integrates the BDD process with Azure DevOps by connecting and synchronizing the BDD scenarios with Test Cases and by publishing test execution results to Azure DevOps in a way that the test result remains connected to the related Test Case. With this the development and testing activities remain transparent and traceable for all stakeholders in Azure DevOps.

SpecSync supports any tool that uses the Gherkin language for describing the BDD scenarios, for example SpecFlow, Reqnroll or Cucumber (see more details in the [Compatibility](reference\compatibility.md) page). 

SpecSync helps people to stay in sync with Azure DevOps and Jira since 2016 and by now it is used by many organizations around the globe.

{% hint style="success" %}
**Your Data in Your Hands**

SpecSync makes direct connection between the machine that runs SpecSync and the remote Azure DevOps or Jira server. The scenario or Test Case data is **not** transferred to or processed by premises of Spec Solutions.
{% endhint %}

Project site: [https://www.specsolutions.eu/specsync/](https://www.specsolutions.eu/specsync/).

This documentation contains the necessary information to configure and use SpecSync. If you are new to SpecSync, we recommend starting with the [Getting Started Guide](getting-started/).

