# Introduction to SpecSync for TFS {#specsync-for-mtm-documentation}

Many teams being currently in a transformation process towards using agile and BDD have to face the problem that the new specification artefacts and reports have to be integrated into the existing tool chain. Many of such teams use Microsoft TFS \(Team Foundation Server / Visual Studio Team Services / Azure DevOps\). As the manual test activities \(especially exploratory testing\) and traceability also play an important role in agile testing and BDD, TFS might have its place in the BDD tool chain. In some circumstances, TFS can also run automated test cases as part of the build/deployment pipeline or in an ad-hoc fashion. These tests can even be executed in dedicated virtual environments so that automated acceptance testing can become more stable and predictable. 

_SpecSync for TFS_ is a synchronization tool that synchronizes Given/When/Then-based BDD scenarios to TFS. It supports any tool that uses the Gherkin language for describing the BDD scenarios, for example SpecFlow or Cucumber.  

Project site: [http://speclink.me/specsync](http://speclink.me/specsync).

More information about the motivation and goals of the project can be found in an earlier post at [http://gasparnagy.com/2016/02/integrating-specflow-with-microsoft-test-manager-mtm/](http://gasparnagy.com/2016/02/integrating-specflow-with-microsoft-test-manager-mtm/).

This documentation contains the necessary information to configure and use SpecSync. If you are new to SpecSync, we recommend starting with the [Getting Started Guide](getting_started.md).
