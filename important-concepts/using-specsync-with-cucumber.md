# Using SpecSync with Cucumber

SpecSync can synchronize any scenarios that are written in Gherkin format. Gherkin format is used by many tools in many platforms, like Cucumber, Cucumber JVM, Cucumber.js, Behat, Behave and also SpecFlow.

If your scenarios are automated with a tool other than SpecFlow, SpecSync will synchronize them as non-automated Azure DevOps Test Cases, because currently Azure DevOps only supports specifying .NET automation for the test cases. The synchronized non-automated test cases can be managed, linked and structured in Azure DevOps. You can also run them manually.

The SpecSync synchronization tool can be executed as a command line tool from Windows, OSX and Linux-based systems. See [Using SpecSync on OSX/Linux](using-specsync-on-osxlinux-page.md) page for details.

For a detailed guide on how to setup and run SpecSync with Cucumber, please check the [Getting started using Cucumber or other Gherkin-based BDD tool](../getting-started/getting-started-cucumber.md) guide.

