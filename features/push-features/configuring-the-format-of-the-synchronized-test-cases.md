# Configuring the format of the synchronized test cases

The format of the synchronized test cases can be changed with the following ways. The format settings can be configured in the [`synchronization/format`](../../reference/configuration/configuration-synchronization/configuration-synchronization-format.md) section of the SpecSync [configuration file](../../reference/configuration/).

* To synchronize `Then` steps to the expected results column of the test case steps, change the `synchronization/format/useExpectedResult` setting.
* To synchronize data tables as plain text, change the `synchronization/format/syncDataTableAsText` setting.
* To skip adding the _Background:_ prefix for background steps, change the `synchronization/format/prefixBackgroundSteps` setting.
* To skip adding the _Scenario:_ or _Scenario Outline:_ prefix for the test case title, change the `synchronization/format/prefixTitle` setting.
* To change the behavior of how scenario outline parameters are synchronized change the `synchronization/format/showParameterListStep` setting.
