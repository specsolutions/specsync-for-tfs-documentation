# Changelog

## v2.1.0

### Breaking changes

* When synchronizing automated test cases, a test execution strategy has to be configured in the [synchronization/automation](configuration/configuration-synchronization/configuration-synchronization-automation.md) section of the configuration file. To read more about test execution strategies see the [Synchronizing automated test cases](important-concepts/synchronizing-automated-test-cases.md) article.

### New features

* Support for SpecFlow v3 both .Net Framework and .Net Core
* New test execution strategy for automated test cases: Assembly based execution
  * Supports all test runners \(MsTest, xUnit, NUnit, SpecFlow+ Runner\)
  * Supports both .NET and .NET Core
  * Available for TFS 2017 or newer
  * See more in the [Synchronizing automated test cases](important-concepts/synchronizing-automated-test-cases.md) article
* Updated "Test Suite Based execution strategy": now supports MsTest, xUnit and NUnit in Azure DevOps \(for MsTest, Scenario Outline wrapper and SpecSync SpecFlow plugin is needed.
  * See more in the [Synchronizing automated test cases](important-concepts/synchronizing-automated-test-cases.md) article
* Allow overriding base folder using the `--baseFolder` console option
* Allow specifying Test Plan ID to be able to improve Test Suite loading time
* Automatically create Test Suite if does not exists

### Bug Fixes:

* Fix displaying load project source in console
* Fix: Invalid "\_MsTest" suffix might be added to the automated test class name
* Better error reporting for project load error
* Fix project URL error message
* Fix changing link type

## v2.0.0

### Breaking changes

* The product has been renamed to SpecSync for Azure DevOps to be conform with the updated product name of Microsoft TFS/VSTS
* The NuGet package has been renamed and split:
  * `SpecSync.AzureDevOps` - contains the synchronization tool
  * `SpecSync.AzureDevOps.SpecFlow.2-3`, `SpecSync.AzureDevOps.SpecFlow.2-4`, etc. - contains the SpecFlow plugin required to [synchronized automated test cases](important-concepts/synchronizing-automated-test-cases.md) for a particular SpecFlow version \(eg. v2.3, v2.4\).
  * `SpecSync.MTM` - obsolete package, will load SpecSync.AzureDevOps, SpecSync.AzureDevOps.SpecFlowPlugin has to be added manually if necessary
* The synchronization tool has been renamed from `SpecSync4MTM` to `SpecSync4AzureDevOps`
* The configuration of the synchronization has been moved to a configuration file \(although a few options can be overridden from the command line\). By default the config file should be named as `specsync.json`. See configuration options and samples at [Configuration](configuration/). You can also check the `specsync-sample.json` file in the `docs` folder of the NuGet package.
* The two-way synchronization has been reworked to be able to support a more stable development process. The synchronization has been split into two independent actions: _push_ and _pull_. The new process should follow a pull-merge/resolve-verify-push model. See [Two-way synchronization](important-concepts/two-way-synchronization.md) for details.
* Tag filtering has been split to _tag filter_ and _tags scope_. Tag filter has to be specified on the command line \(`--tagFilter`\), while tag scope has to be specified in the config file \(`local/tags`\). See [Filters and scopes](important-concepts/filters-and-scopes.md) for details.
* If a scenario was linked to a test case that does not exist, SpecSync reports this as an error \(in v1 it linked to a new scenario and replaced the wrong tag\)
* The 'state' field of the test case is set to the value provided in configuration `synchronization/state/setValueOnChangeTo` not only when the test case changed \(v1\) but also when the test case is created.
* The default way of generating test method wrappers has changed to `iterateThroughExamples`. See [`specFlow` configuration](configuration/configuration-specflow.md) for details.

### New features

* Support for specifying test case field default values and custom field updates. See [`customizations` configuration](configuration/configuration-customizations.md).
* Support for skipping the `Scenario` or `Scenario Outline` prefixes in test case title. See [`synchronization` configuration](configuration/configuration-synchronization/).
* Test case tag prefix name \(by default `tc`\) can be configured. See [`synchronization/format` configuration](configuration/configuration-synchronization/configuration-synchronization-format.md).
* Support for ignoring test case steps with a specific prefix text. See [`customizations` configuration](configuration/configuration-customizations.md).
* Support for synchronization of scenarios on a feature branch. See [`customizations` configuration](configuration/configuration-customizations.md).
* Support for folder-based synchronization. See [`local` configuration](configuration/configuration-local.md).
* SpecSync can track format configuration changes and automatically re-sync scenarios without the `--force` option.
* More readable sync trace output. 

## v1.6.0 - 2018/07/25

* Support for SpecFlow v2.3.\*
* Support for synchronizing scenarios from a branch to a temporary set of test cases \(see `--branchTagPrefix` option, experimental\)
* Fix: Generating feature-file code-beind files fail with the SpecFlow Visual Studio integration v2017.1.11 or later.
* Fix: Do not remove filtered out test cases from the test suite.
* Fix: Invalid tools version error when back-syncing new test cases.
* Fix: Handle `&nbsp;` in test cases.
* Fix: Use proper space encoding in the URL specified in the `@tfs` tag.
* Fix: Do not add `@tfs` tag to feature if otherwise not changed \(scenarios skipped\)
* Fix: Space handling in TFS collection name.
* Fix: Missing parameter in paremetrized test case if the Scenario Outline placeholder is used in a DataTable.
* Improved error handling

## v1.5.0 - 2018/04/11

* Support for SpecFlow v2.3.1 \(for SpecFlow v2.2.1, please use v1.4.\*\)

## v1.4.1 - 2018/04/10

* Fix: ArgumentOutOfRangeException when syncronizing empty feature file
* Fix: Do not count skipped scenarios in summary
* Improved error handling

## v1.4.0 - 2018/03/20

* Use new TFS REST API \(supports TFS 2015+\)
* Support synchronizing non-SpecFlow projects with the `--listFilePath` option. See page [Using SpecSync with Cucumber](http://speclink.me/specsynccucumber) for details.
* Allow executing the synchronzation from OSX and Linux systems. See page [Using SpecSync on OSX/Linux](http://speclink.me/specsyncosxlinux) for details.
* Filter scenarios to be synchronized with [tag expressions](https://docs.cucumber.io/tag-expressions/) using the `--tags` option.
* `--skipAutomation` supports [tag expressions](https://docs.cucumber.io/tag-expressions/).
* Improved test case formatting options \(see [Configuring the format of the synchronized test cases](important-concepts/configuring-the-format-of-the-synchronized-test-cases.md)\)
  * Synchronize `Then` steps to the _expected results_ column of the test case steps by specifying the `--useExpectedResult` option.
  * Synchronize data tables as plain text by specifying the `--syncDataTableAsText` option.
  * Skip adding the _Background:_ prefix for background steps by specifying the `--doNotPrefixBackgroundSteps` option.
  * Force synchronization even if the scenario text did not change by specifying the `--force` option. This is useful after changing formatting options like `--useExpectedResult`.
* Better handling invalid Gherkin files.
* Generate `@tc:123` tags on new lines \(before the first existing tag line\).
* Update existing \(invalid or incomplete\) `@tc:` tags instead of generating a new one.

## v1.3.2 - 2017/10/31

* Fix SpecFlow+ Runner compatibility issue for Scenario Outlines

## v1.3.1 - 2017/10/10

* Feature: Specify an area or an iteration for the newly created test cases using `--areaPathForCreate` and `--iterationPathForCreate`options See [Add new test cases to an Area or an Iteration](https://gasparnagy.gitbooks.io/specsync-for-mtm-documentation/content/add-new-test-cases-to-an-area-or-an-iteration.html) for details. 
* Upgrade to SpecFlow 2.2.1.
* Fix project loading issues.
* Fix SpecFlow+ Runner integration issue.
* Fix Newtonssoft.Json loading issue.
* Fix IndexOutOfBounds error when synchronizing data tables with empty cells.

## v1.2.0 - 2017/03/31

* Support synchronizing '@us:123' style tags to links between the test case and other work items. The tag prefixes and the link type can be specified. See [Linking work items using tags](important-concepts/linking-work-items-with-tags.md) for details.
* Small fixes in Scenario Outline table handling
* Fix compatibility issue for running synchronized test cases using the TFS lab environment.
* Add application icon

## v1.1.6 - 2016/08/11  <a id="v1-1-0"></a>

* Fix authorization problem with VSTS for Alternate authentication credentials and Personal access tokens
* Set test case tags to the tags of the scenario
* Only update changed test cases
* Synchronize automation does not need to have a compiled test assembly
* Feature files are automatically regenerated if changed
* Optionally set test case state to a configured value when test case is updated
* Synchronize test cases to test suites
* Support for SpecFlow+ Excel examples
* More robust error handling
* Two-way synchronization \(beta\)

## v1.0.1 - 2016/03/17  <a id="v1-0-1"></a>

* Only display the exception messages by default, you can use the `--verbose` option to get the stack trace.
* The tool returns with non-zero exit code if there was an error.
* The ESC key can be used to cancel password entering.
* Allow specifying environment variables in user name.
* Fix tag placing issue.
* Allow more authentication options \(sign-in prompts, domain account\)
* Tested with TFS 2013.

## v1.0.0 - 2016/02/24  <a id="v1-0-0"></a>

Initial version used in the demo video in the [intro post](http://speclink.me/spsintro).

