# Using SpecSync with SpecFlow+

SpecSync can synchronize scenarios from projects that use SpecFlow+. SpecFlow+ is a set of commercial extensions and tools for SpecFlow provided by TechTalk. You can find further information about SpecFlow+ at [http://www.specflow.org/plus](http://www.specflow.org/plus).

## Integration with SpecFlow+ Runner

SpecFlow+ Runner \(formerly known as "SpecRun"\) is a test runner specialized to execute SpecFlow scenarios. Azure DevOps cannot run SpecFlow+ Runner test methods through test cases, but SpecSync can be used to synchronize non-automated test cases even if you use this test runner. \(For automated test cases, MsTest has to be used. See [Synchronizing automated test cases](synchronizing-automated-test-cases.md) for more details.\)

_Note: In earlier versions of SpecFlow+ Runner there was an option to generate an additional set of test method using another test runner \(like MsTest\). With that SpecSync was able to generate the necessary test methods to associate with the automated Azure DevOps test case. This feature has been removed from SpecFlow+ Runner due to the new plugin handling infrastructure of SpecFlow, so this option not available anymore. If you need a custom solution to be able to use SpecSync with SpecFlow+ Runner to generate automated test cases, please contact support at specsync@specsolutions.eu._

## Integration with SpecFlow+ Excel

SpecFlow+ Excel can be used to edit the scenarios or the scenario outline example tables in Excel. See [http://www.specflow.org/plus/excel/](http://www.specflow.org/plus/excel/) for details.

SpecSync can synchronize SpecFlow+ Excel scenarios where Excel is used for storing the example tables.

The synchronization of Excel feature files has limited support. SpecSync can synchronize the updates, but the tags necessary for the initial lining have to be added to the Excel file manually. If this limitation would be problematic for you, please contact support.

