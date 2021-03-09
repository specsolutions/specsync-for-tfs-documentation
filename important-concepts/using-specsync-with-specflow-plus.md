# Using SpecSync with SpecFlow+

{% hint style="warning" %}
This documentation page is outdated and represents SpecSync v2. Please check the page later for an updated version.
{% endhint %}

SpecSync can synchronize scenarios from projects that use SpecFlow+. SpecFlow+ is a set of commercial extensions and tools for SpecFlow provided by TechTalk. You can find further information about SpecFlow+ at [http://www.specflow.org/plus](http://www.specflow.org/plus).

## Integration with SpecFlow+ Runner

SpecFlow+ Runner \(formerly known as "SpecRun"\) is a test runner specialized to execute SpecFlow scenarios. SpecSync can be used to synchronize test cases from Scenarios in projects using the SpecFlow+ Runner.

In order to publish test results executed with SpecFlow+ Runner, the runner needs to be invoked with `dotnet test` and the generated TRX file can be published with SpecSync as described in [How to synchronize automated test cases](synchronizing-automated-test-cases.md).

## Integration with SpecFlow+ Excel

SpecFlow+ Excel can be used to edit the scenarios or the scenario outline example tables in Excel. See [http://www.specflow.org/plus/excel/](http://www.specflow.org/plus/excel/) for details.

SpecSync can synchronize SpecFlow+ Excel scenarios where Excel is used for storing the example tables.

SpecFlow+ Excel is not supported by recent SpecFlow versions, so the support of this format had to be removed from SpecFlow in v3.0.

