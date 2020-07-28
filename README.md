# Introduction to SpecSync for Azure DevOps

Many teams being currently in a transformation process towards using agile and BDD have to face the problem that the new specification artifacts and reports have to be integrated into the existing tool chain. Many of such teams use Microsoft Azure DevOps \(Team Foundation Server - TFS / Visual Studio Team Services - VSTS\) where the concept of Test Cases and Test Runs can be used to represent and trace the testing activities. 

_SpecSync for Azure DevOps_ integrates the BDD process with Azure DevOps by connecting and synchronizing the BDD scenarios with Test Cases and by publishing test execution results to Azure DevOps in a way that the test result remains connected to the related Test Case. With this the development and testing activities remain transparent and traceable for all stakeholders in Azure DevOps.

SpecSync supports any tool that uses the Gherkin language for describing the BDD scenarios, for example SpecFlow or Cucumber.

Project site: [https://www.specsolutions.eu/services/specsync/](https://www.specsolutions.eu/services/specsync/).

More information about the motivation and goals of the project can be found in an earlier post at [http://gasparnagy.com/2016/02/integrating-specflow-with-microsoft-test-manager-mtm/](http://gasparnagy.com/2016/02/integrating-specflow-with-microsoft-test-manager-mtm/).

This documentation contains the necessary information to configure and use SpecSync. If you are new to SpecSync, we recommend starting with the [Getting Started Guide](getting-started/).

