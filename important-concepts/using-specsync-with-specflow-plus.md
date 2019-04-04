# Using SpecSync with SpecFlow+

SpecSync can synchronize scenarios from projects that use SpecFlow+. SpecFlow+ is a set of commercial extensions and tools for SpecFlow provided by TechTalk. You can find further information about SpecFlow+ at [http://www.specflow.org/plus](http://www.specflow.org/plus).

## Integration with SpecFlow+ Runner

SpecFlow+ Runner \(formerly known as "SpecRun"\) is a test runner specialized to execute SpecFlow scenarios. SpecSync can be used to synchronize test cases from Scenarios in projects using the SpecFlow+ Runner.

For associating automated test results of SpecFlow+ Runner with the synchronized test cases, the "Assembly based execution" strategy can be used as described in the [Synchronizing automated test cases](synchronizing-automated-test-cases.md) article.\)

## Integration with SpecFlow+ Excel

SpecFlow+ Excel can be used to edit the scenarios or the scenario outline example tables in Excel. See [http://www.specflow.org/plus/excel/](http://www.specflow.org/plus/excel/) for details.

SpecSync can synchronize SpecFlow+ Excel scenarios where Excel is used for storing the example tables.

The synchronization of Excel feature files has limited support. SpecSync can synchronize the updates, but the tags necessary for the initial lining have to be added to the Excel file manually. If this limitation would be problematic for you, please contact support.

