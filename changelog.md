# Changelog

{% hint style="info" %}
The [How to upgrade to a newer version of SpecSync](important-concepts/how-to-upgrade-specsync.md) guide contains information about how to upgrade to a newer version.
{% endhint %}

{% hint style="info" %}
For planned features in future releases please check the [Release Model and Roadmap](roadmap.md) page.
{% endhint %}

## v3.4.0 - coming soon

### New features

* Support updating Test Case fields that are not updated automatically by SpecSync using the [`fieldUpdates` configuration](reference/configuration/configuration-synchronization/configuration-synchronization-fieldupdates.md). See [feature description](features/push-features/update-test-case-fields.md) for details. (#737)
* In all expressions where you can specify a local test case condition (`synchronization/automation/condition`, `customizations/resetTestCaseState/condition`, `synchronization/fieldUpdates[]/condition`, `synchronization/fieldUpdates[]/conditionalValue/*`), you can now also specify source file path conditions as well using the `$sourceFile ~ path/**/myfeature.feature` format. E.g. the following expression selects scenarios that are tagged with `@important` and are in the `pricing` folder or any sub-folders below: `@important and $sourceFile ~ pricing/`. See [Local test case conditions](features/general-features/local-test-case-conditions.md) for details. (#878)
* Add/remove link tags on pull when Test Case links changed. SpecSync attempts to use the best tag prefix if there are multiple available by selecting the first with matching `targetType` or the first of the ones without `targetType` otherwise. (#807)
* SpecSync plugins can now be loaded from NuGet packages that are distributed through nuget.org, custom NuGet feeds or from local folders. See [SpecSync plugin documentation](features/general-features/specsync-plugins.md) for examples and further details. There is also a [list of plugins](features/plugin-list.md) available on nuget.org. (#814)
* The tag prefix separator (`:`) and the link label separator (`;`) can be customized now with `synchronization/tagPrefixSeparators` and `synchronization/linkLabelSeparator`. With that you can allow tags like `@TestCase=1234`. You can specify multiple tag prefix separators as well. SpecSync will use the first one to create new tags. (#773)
* Allow skip publishing test results for local test cases using the `--tagFilter` and `--sourceFileFilter` options. (#853)
* For custom placeholder in field updates, different "value loaders" can be specified. Value loaders can transform the value. E.g. if the `HTML` loader is used in a field update as `{scenario-description:HTML}`, it will replace the white space and new line characters of the scenario description with the necessary HTML elements. The following value loaders are supported: (#781)
  * `HTML` - transforms whitespaces to HTML newlines and non-breaking spaces.
  * `Unix` - replaces Windows-style path separators (`\`) with Unix-style ones (`/`)
  * `Windows` - replaces Unix-style path separators (`/`) with Windows-style ones (`\`)
* Support Cucumber Java Surefire XML report (#795)
* Support for SpecFlow v4.0 (#901)
* Support for .NET 7: The [.NET tool installation](installation/dotnet-core-tool.md) now works on systems that only have .NET 7.0 SDK. (#868)

### New Plugins

See all plugins available on nuget.org in the [plugin list](features/plugin-list.md)

* **SpecSync.Plugin.ScenarioOutlinePerExamplesTestCase**: Plugin that synchronizes scenario outline examples blocks as individual test cases. See plugin details and usage [in the SpecSync plugins GitHub repository](https://github.com/specsolutions/specsync-sample-plugins#scenario-outline-per-exampes-test-case-plugin) (#718)
* **SpecSync.Plugin.ExcelTestSource**: This plugin can be used to synchronize a local test cases from Excel file using the format that Azure DevOps uses when you export Test Cases to CSV. See plugin details and usage [in the SpecSync plugins GitHub repository](https://github.com/specsolutions/specsync-sample-plugins#excel-test-source-plugin)
* **SpecSync.Plugin.MsTestTestSource**: Allows synchronizing "C# MsTest Tests" and publish results from TRX result files. See plugin details and usage [in the SpecSync plugins GitHub repository](https://github.com/specsolutions/specsync-sample-plugins#mstest-test-source-plugin)

### Improvements

* The SpecSync Docker image is now based on the latest stable Ubuntu image (`ubuntu:22.04`) (#696)
* SpecSync now retries saving the Test Case if it has been modified while processing. (#614)
* Unrecognized elements in the configuration files are reported as warnings. (#806)
* Do not report items that are skipped because of scope by default. Using the verbose option (`--verbose` or `-v`) reports the skipped items as well. (#818)
* The user-specific configuration file got a separate JSON schema: `https://schemas.specsolutions.eu/specsync-user-config-latest.json` (#859)
* The type of the linked work items can be restricted using the `synchronization/link[]/targetType` or the `customizations/linkOnChange/links[]/targetType` configuration settings. (#822, #867)
* Filter, scope and other conditions are satisfied if there is at least one scenario outline examples that satisfies the condition. (#717)
* Improve performance of filtered / scoped command executions (#856)
* Detecting non-interactive user session for operations require user input and automatically cancel to avoid blocking process (#891)
* Improved publish-test-result reporting (#915)
* Improve progress indication for publish test results (#889)
* The test results with not executed outcome (`NotExecuted`, `NotApplicable`, `NotRunnable`, `NotImpacted`) are not published by default as they might hide earlier results of the Test Case. In order to force including them, the `publishTestResults/includeNotExecutedTests` setting can be used. (#894)
* The product name (SpecSync for Azure DevOps or SpecSync for Jira) is visible in the license file when opened in a text editor. (#876)
* Various stability and maintainability improvements (#911, #837, #808, #898, #880, #874, #834, #821, #832)

### Deprecation notices <a href="v3-4-0-deprecation" id="v3-4-0-deprecation"></a>

* The configuration setting `publishTestResults/ignoreNotExecutedTests` has been deprecated as ignoring test results with not executed outcome is the default now. (In order to force including them, the `publishTestResults/includeNotExecutedTests` setting can be used.) (#894)
* The configuration settings `local/featureFileSource/type`, `local/featureFileSource/filePath` and `local/featureFileSource/folder` are moved to the `local` group and renamed to `local/projectType`, `local/projectFilePath`, `local/folder`. The old settings work, but show a warning. (#873)
* The .NET 5 framework is out of support and will not receive security updates in the future (see https://aka.ms/dotnet-core-support). SpecSync versions released after 31/3/2023 will not run with .NET 5. Please use SpecSync with .NET 6 or any of the other supported platforms. (#919)
* The configuration setting `synchronization/link[]/targetWorkItemType` has been renamed to `synchronization/link[]/targetType` and SpecSync now verifies if a valid work item type has been specified when linking. The old setting works, but shows a warning. The additional verification is performed even if the old setting was used. (#822)
* The configuration settings `toolSettings/testCaseWorkItemName` and `toolSettings/testSuiteWorkItemName` are moved to `/remote/azureDevOps/testCaseWorkItemName` and `remote/azureDevOps/testSuiteWorkItemName`. The old settings work, but show a warning. (#862)
* The configuration setting `customizations/synchronizeLinkedArtifactTitles/linkLabelSeparator` has been moved to `synchronization/linkLabelSeparator`. The old settings work, but show a warning. (#850)
* The configuration setting `publishTestResults/createSubResults` has been removed, because SpecSync always needs to publish sub-results anyway for the proper test result reporting in Azure DevOps. (#801)
* The configuration settings `publishTestResults/runName`, `publishTestResults/runComment`, `publishTestResults/runType`, `publishTestResults/testResultComment` have been moved to `publishTestResults/testRunSettings/name`, `publishTestResults/testRunSettings/comment`, `publishTestResults/testRunSettings/runType` and `publishTestResults/testResultSettings/comment`. The old settings work, but show a warning. (#804)


### Breaking changes <a href="v3-4-0-compatibility" id="v3-4-0-compatibility"></a>

* Settings that have been deprecated earlier are removed now (#805)
  * `automation/skipForTags` - use `automation/condition` instead
  * `links[]/mode` - was not used
  * `local/featureFileSource/type=listFile` - use `folder` as `local/projectType` in combination with `local/sourceFiles` instead
  * `local/featureFileSource/type=stdIn` - use `folder` as `local/projectType` in combination with `local/sourceFiles` instead
* If the configuration file contained entries that are not recognized by SpecSync (these were ignored so far), you will receive warnings about these settings. Please remove these settings to eliminate the warning. (#806)
* The tag prefixes specified in `synchronization/links[]/tagPrefix` are ensured to contain only word characters (letters, numbers, underscore). (#849)
* The default link-label separator has been changed from `:` to `;`. To continue using `:` as link label separator, please set `synchronization/linkLabelSeparator`. (#861)
* The support for [Test Plan / Test Suite based test execution](features/test-result-publishing-features/support-for-azure-devops-test-plan-test-suite-based-test-execution.md) for SpecFlow 3.0 has been removed. SpecSync can still be used with any SpecFlow versions using the ["publish-test-results" command](features/test-result-publishing-features/publishing-test-result-files.md) (#903)
* The test results with not executed outcome are not published by default. In order to force including them (the earlier behavior), the `publishTestResults/includeNotExecutedTests` setting can be used. (#894)
* Tag predicates in local test case conditions must start with '@'. Earlier tag predicates were also accepted, but from this version the `@` has to be used, even if the local test case does not use this prefix for tags. (#877)
* The value specified as `synchronization/link[]/targetWorkItemType` (now renamed to `synchronization/link[]/targetType`) is used now to verify if the type of the linked work item. The verification is performed when establishing new links. To disable the verification and allow linking any work item types please remove this setting. (#822)
* Test Suite settings cannot be specified in user-specific configuration file anymore. The can be specified in project-specific configuration files or shared parent config files still. (#863)
* The "pull" command now adds and removes local test case tags based on the links of the Test Case. (#807)

### Plugin API improvements <a href="v3-4-0-plugin-api" id="v3-4-0-plugin-api"></a>

* SpecSync plugins can now be loaded from NuGet packages that are distributed through nuget.org, custom NuGet feeds or from local folders. (#814)
* SpecSync plugins will support [SpecSync for Jira](https://speclink.me/specsync-jira) as well, from the next SpecSync for Jira version (v1.2). (#902)
  * The plugin dependency package has been renamed from `SpecSync.AzureDevOps.PluginDependencies` to `SpecSync.PluginDependencies`.
  * The plugin API namespace has been renamed from `SpecSync.AzureDevOps.*` to `SpecSync.*`. (#833)
* Plugins can change the configuration via the `PluginInitializeArgs.Configuration` property (#857)
* Various improvements in plugin API (#857, #809)
  * `BddProjectLoaderArgs.FeatureFileSource` has been renamed to `BddProjectLoaderArgs.LocalConfiguration`
  * `TagServices` is available as `ITagServices`
  * `ISpecSyncTracer.TraceWarning` signature now contains a single `TraceWarningItem` parameter. (#815)
  * `IKeywordParser` can signal parsing errors and no keyword cases: the method `ParseStepKeyword` has been replaced by `TryParseStepKeyword`. (#792)
  * Link tags now can contain non-numeric values as well
  * `ITagServices.GetTagData` requires `ITestCaseSyncContext` instead of `ILocalTestCase`
  * `LinkData.WorkItemId` (`int`) has been replaced by `LinkData.WorkItemIdentifier` (`WorkItemIdentifier`)
  * `LinkData.IsSpecSyncLink` is replaced by `LinkData.IsTrackedLink`
  * `LinkData` default constructor has been removed (related properties are get-only)
  * `TestCaseLink` constructor takes `TestCaseIdentifier` instead of `int`
  * `DotNetProjectLoader.CreateProjectReader` takes `BddProjectLoaderArgs` argument
  * `DotNetProjectReader` and `SpecFlowProjectReader` constructor takes `ISpecSyncTracer` argument
  * `IKeywordParser` has a new method: `GetPrimaryLocalTestCaseParametersKeyword`
* Plugin dependencies are automatically loaded from the plugin folder (#908)
* Support for custom automation details with plugins by implementing the `IAutomationSettingsProvider` interface on the `ILocalTestCase` implementation (#909)
* Improvements in plugin dependency `SpecSync.AzureDevOps.PluginDependency.CSharpSource`:
  * Support for updating links in C# files. (#851)
  * Support for synchronizing descriptions from C# doc comments. (#843)

### Bug Fixes

Fix: Wait on stats calls block execution (#914)
Fix: Plugins cannot be loaded with console app (#910)
Fix: File paths in tags are not normalized on Linux and macOS (#912)
Fix: Different hash is calculated when running on Windows or macOS/Linux causing unnecessary updates (#881)
Fix: Plugins might be reported twice (#864)
Fix: Pull command generates invalid scenario outlines for non-English feature files (#811)

## v3.3.11 - 2022/12/19

### Bug fixes

* Fix: Scenario test results of pytest-playwright are not found (#917)

## v3.3.10 - 2022/12/12

_Note: The release v3.3.9 has been revoked due to a release issue._

### Bug fixes

* Fix: Push command fails when invoked with --dryRun, a Test Suite is configured and there are multiple unlinked scenarios (#900)
* Fix: Cucumber JUnit XML report does not find additional scenario outline examples with recent Cucumber version (#796)
* Fix: Cucumber JSON scenario outline results are incorrectly detected as re-runs (#904)
* Fix: Cucumber test results might not be detected when scenario outline placeholder is used in the title (#472)

## v3.3.8 - 2022/12/07

### Bug fixes

* Fix: Configuration files are reported twice when configOverride is used (#890)
* Fix: Background step results might not be loaded from Cucumber JSON result (#819)
* Fix: local/featureFileSource setting cannot be overridden in a child config file (#798)
* Fix: Diagnostic output for configuration is not available (#797)
* Fix: Invalid Gherkin file created after pulling a Test Case with a "|" in a data table (#817)
* Fix: SpecSync might ask for password when version command is used (#848)
* Fix: Configuration override values cannot contain equals sign (#770)
* Fix: SpecSync Plugins: `NoKeywordParser` produces invalid TestSourceData (#791)

### Improvements

* SpecSync NuGet packages are signed with Code Signing Certificate of Spec Solutions Kft., thumbprint: dd5e69f05ef38c016508380ca0d2294dbbe1ba69 (#882)
* Allow not publishing test results that are not executed using publishTestResults/ignoreNotExecutedTests (#893)
* Package for common C# test source plugin dependencies: `SpecSync.AzureDevOps.PluginDependency.CSharpSource` (#793)
* Allow using Client Certificates automatically from Windows Credential store for remote servers use mTLS (#838)

## v3.3.7 - 2022/08/24

### Bug fixes

* Fix: Indentation is not kept when tagging scenarios (#754)
* Fix: Relation already exists error when linking Azure DevOps pull requests (#762)

### Improvements

* Support linking GitHub Pull Requests (#761)

## v3.3.6 - 2022/07/13

### Bug fixes

* Fix: Duration in Cucumber JSON results is interpreted incorrectly (#748)
* Fix: Publishing tests executed by the VsTest re-run setting is not supported. (This fix changes the default for the `publishTestResults/createSubResults` to `true`.) (#745)

### Improvements

* Allow specifying empty "Action" value (#734)

## v3.3.5 - 2022/06/09

### Bug fixes

* Fix: Synchronize linked artifact titles customization does not link scenario if it contains only a requirement link tag (#731)

### Improvements

* Treat test result attachment upload errors as warnings: Ignore test result attachments that are too large (#727)
* Allow placeholders (`{testrun-id}`, `{testrun-url}`) in test run and test result comment (#724)
* Show created Test Run URL in output (#732)

## v3.3.4 - 2022/05/13

### Bug fixes

* Fix: XML/HTML tags replaced to parameters in normal scenarios (not scenario outlines) (#712)
* Fix: Handle PAT expiration GSSAPI error as normal error (#715)
* Fix: .NET projects with special wildcard includes cannot be processed (#720)
* Fix: The reported parameter usages are shifted in the published results when parameter list pseudo step is used (#716)

### Improvements

* [Customization: Synchronize linked artifact titles](features/push-features/customization-sync-linked-artifact-titles.md). This customization can be used to synchronize linked artifact (Work Item) titles back to the local test case tags in `@story:1234:This_is_the_story_title` format. The link tags are only updated when the scenario is otherwise changed or when the `--force` option is used. (#714)

## v3.3.3 - 2022/05/04

### Bug fixes

* Fix: Allow disabling release pipeline association for publish-test-result command with the new `--disablePipelineAssociation` command line option. (#707)

### Improvements

* Allow enabling timing diagnostics (#708)

## v3.3.2 - 2022/04/28

### Bug fixes

* Fix: Strong name validation failed error when using the SpecSync.AzureDevOps.Console package (#695)
* Fix: Init command does not work when config file name is specified in the command line (#686)

### Improvements

* Allow specifying additional work item clones for re-link in a CSV file using the `--workItemClonesFile` option. (#663)
* Display .NET version information in verbose mode (#694)
* The SpecSync Docker images now provide a `specsync` executable for simpler usage (it is a symbolic link to the executable `SpecSync4AzureDevOps`). (#682)
* The SpecSync Docker images are now based on the `ubuntu:20.04` image (#697)
* Smaller improvements for work item identity and console handling (#681, #687)

## v3.3.1 - 2022/03/10

### Bug fixes

* Fix: Test execution history is not displayed in the History tab of the Test Run (#653)
* Fix: Tests appear multiple times in the 'Tests' tab of the pipeline result when the `VsTest` task is used. To fix this there is a new "SpecSync Tools" Azure DevOps extension, that contains a task "Visual Studio Test for SpecSync" that is a modified version of the `VsTest` task that does not display the result in the 'Tests' tab. (See [How to use the SpecSync Azure DevOps pipeline tasks](important-concepts/how-to-use-the-pipeline-tasks.md) for details.) (#655)

## v3.3.0 - 2022/02/21 <a href="v3-3-0" id="v3-3-0"></a>

### New features

* Attach files to Test Cases using tags. (See [feature documentation](features/push-features/attach-files.md) for details.) (#489)
* Dry-run mode: all commands of SpecSync support a `--dryRun` option. With this option used no changes will be made neither to Azure DevOps nor to the feature files. This option is useful for testing the impact of an operation without making an actual change. (#115)
* Re-link: a new SpecSync command that supports re-linking all scenarios after the Test Cases have been cloned using the Azure DevOps "Copy Test Plan" or "Copy Test Suite" features. (See [feature documentation](features/common-synchronization-features/re-link-scenarios.md) for details.) (#499)
* Source files local scope: The set of scenarios considered for synchronization ("local scope") can be now also specified by specifying the set of feature files using the `local/sourceFiles` setting in the configuration file. (See [Excluding scenarios from synchronization](features/common-synchronization-features/excluding-scenarios-from-synchronization.md) for details.) (#587)
* Filter for feature files: when performing a push or pull command, the execution can be limited to a set of feature files using the `--sourceFileFilter` option. (See [Filters and scopes](important-concepts/filters-and-scopes.md) for details.) (#492)

### Improvements

* Display more details about the synchronization command (e.g. how many Test Cases have been modified, how many Test Cases were up-to-date) (#583)
* Link-only flag for push: you can specify a `--linkOnly` flag to the push command, that skips updating the existing Test Cases, but creates and links new Test Cases for the new scenarios. (#633)
* Create-only flag for pull: you can specify a `--createOnly` flag to the pull command, that skips updating the existing scenarios, but creates new scenarios (in new feature files) for the new Test Cases. (#303)
* The "Automated Test Type" Test Case field is better used: The default value (`Unit Test`) for the "Automated Test Type" Test Case field has been changed. The new default value is `Gherkin` for non-SpecFlow projects and `SpecFlow` for SpecFlow projects. The default value of the field can be changed using the `synchronization/automation/automatedTestType` setting in the configuration file. The change is performed with the next change of the Test Case or can be forced by running the push command with an additional `--force` flag. (#550)
* Work item link tracking: The work item links created by SpecSync are tracked now. This means that if a link was established by SpecSync and the related scenario tag is removed from the feature file, the link will also be removed at the next synchronization. The links created manually or by an earlier version of SpecSync are never removed. (#570)
* All scenario outline examples columns are preserved. If you had a scenario outline with an examples table that contained a column (e.g. "description") that was not used anywhere in the steps, this column was not synchronized to Azure DevOps. Now in this case an additional pseudo-test step is added to the Test Case, in order to preserve all parameters. This behavior can be forced to all scenario outlines or completely disabled using the `synchronization/format/showParameterListStep` configuration setting. (#569)
* Support for Gherkin v22: tagged rules, rule-specific backgrounds (#497)
* Support for Background steps in pull (#611)
* Improved change detection for scenarios (new hash algorithm) (#111)
* Improved possibilities for SpecSync plugins
  * Allow analyzing custom keyword (#552)
  * Allow using class name and method name in test result matchers for detecting data row (#603)
  * Customizable base classes for writing custom test source plugins easier (#557)
  * The test result matcher infrastructure has been adapted to the other "provider-style" services and `TestResultMatcher` has been renamed to `TestResultMatcherProvider` on the plugin interface (#647)
  * More functions defined on the `ILocalTestCaseContainerUpdater` interface (#648)

### Breaking changes <a href="v3-3-0-compatibility" id="v3-3-0-compatibility"></a>

* The scenario hash that is calculated for the Test Cases (saved to the history) is not backwards compatible with v3.2 (see #111). 
  This means that once a Test Case has been updated with v3.3, **when downgrading to v3.2** an additional change will be recorded for the Test Cases. With the normal usage (no downgrade) the change has no impact.
* The support for running **SpecFlow v2** scenarios using the [Test Plan / Test Suite based execution](features/test-result-publishing-features/support-for-azure-devops-test-plan-test-suite-based-test-execution.md) has been removed (ie there is no update provided for the related SpecFlow plugins). You can still publish results from SpecFlow v2 projects using the "publish-test-results" command and can use the v3.2 plugins in combination with the v3.3 synchronizer if necessary. (#571)
* As the default value of the "Automated Test Type" Test Case field has been changed (see #550), you need to explicitly set the `synchronization/automation/automatedTestType` setting to `Unit Test` (the old default value), **when your processes were dependent on the old "Automated Test Type" value**. Usually this value is only informational, so the change will have no impact.
* For scenario outlines with examples table that has columns not used in the steps, SpecSync will automatically add a pseudo-step by default to preserve these values (see #569). If this causes any problems, the feature can be disabled by setting the `synchronization/format/showParameterListStep` configuration setting to `never`. 
* The target framework of the SpecSync SpecFlow plugins has been updated to use .NET Framework 4.7.2 instead of 4.7.1. This should have no impact on the usage. (The SpecSync.AzureDevops.Console package still depends on .NET Framework 4.6.2, this wasn't changed.) (#566)
* The `listFile` and the `stdIn` values in the `local/featureFileSource/type` configuration setting are deprecated and will be removed in a future version. It is recommended to use `folder` value in combination with `sourceFiles` setting (#587) instead.
* The `synchronization/links[]/mode` setting is deprecated and will be removed in a future version with the introduction of link synchronization (#570). As the mode had only one possible value, this change will have no impact. Please remove the setting.
* Because of the improved possibilities for SpecSync plugins, the SpecSync plugins created for v3.2 have to be adapted slightly.

### Bug Fixes

* Fix: Feature tags are duplicated at the scenario on pull (#612)
* Fix: Test Case parameters are not cleared when a Scenario Outline is changed to a normal Scenario (#641)
* Fix: When only non-SpecSync managed fields changed in ADO, but these fields are updated with a customization, scenario is detected to be up-to-date even with --force (392)

## v3.2.13 - 2022/03/10

### Bug fixes

* Fix: Test execution history is not displayed in the History tab of the Test Run (#653)
* Fix: Tests appear multiple times in the 'Tests' tab of the pipeline result when the `VsTest` task is used. To fix this there is a new "SpecSync Tools" Azure DevOps extension, that contains a task "Visual Studio Test for SpecSync" that is a modified version of the `VsTest` task that does not display the result in the 'Tests' tab. (See [How to use the SpecSync Azure DevOps pipeline tasks](important-concepts/how-to-use-the-pipeline-tasks.md) for details.) (#655)

## v3.2.12 - 2022/02/07

### Bug fixes

* Fix: Test Configuration might not be resolved by name when there are more than 200 configurations specified in the project (#629)
* Fix: The relativeResultsDirectory attribute of the UnitTestResult element of the TRX file is not used to find attachments (#635)

## v3.2.11 - 2022/01/18

### Bug fixes

* Fix: Scenario outline xUnit result might not be found in TRX file when more data is provided in the examples table (#623)
* Fix: String values cannot be set wrapped by quotes for config override (#610)
* Fix: Config file path is not included in JSON deserialization error messages (#535)

## v3.2.10 - 2021/12/16

### Bug fixes

* Fix: When the specified Test Suite does not exists, an unnecessary ADO request is made (#582)
* Fix: Scenario Outline results cannot be published when SpecFlow generator is configured with allowRowTests=false for xUnit and NUnit (#602)

## v3.2.9 - 2021/12/08

### Improvements

* The [Ignore non-supported local tags](features/push-features/customization-ignore-non-supported-local-tags.md) customization has been extended to allow specifying the not-supported tags instead of the supported tags in order to handle cases where you want to skip synchronizing certain tags. (#568)
* Override any configuration setting from command line using the `--configOverride` option. See [Override configuration setting from command line](reference/command-line-reference#override-configuration-setting-from-command-line) for details. (#578)

## v3.2.8 - 2021/11/22

### Improvements

* Support for .NET 6 framework when [installed as a .NET tool](installation/dotnet-core-tool.md). (#565)

## v3.2.7 - 2021/11/04

### Improvements

* [Customization: Ignore non-supported local tags](features/push-features/customization-ignore-non-supported-local-tags.md). This customization is useful when the Azure DevOps settings does not allow creating new tags for normal users. With the customization you can specify those tags that are supported in the Azure DevOps project and SpecSync will ignore all other local (scenario) tags. (#538)
* Display currently enabled customizations on the output (#545)
* Allow specifying value for the Automated Test Type field of the Test Case (to override the default `Unit Test`) and be able to select `custom` as automation strategy (useful for non .NET projects) (#549)
* Show error if multiple scenarios are linked to the same Test Case (#546)
* Show error if a scenario has multiple Test Case tags (#553)
* Support for NUnit v2 XML test result file format (#554)

### Bug fixes

* Fix: Additional space included to the Test Case title or parts of the title is missing when the local test case title contains a colon and synchronized from a plugin (#551)
* Fix: JSON schema for the configuration file does not allow `:` in tag expressions (#536)

## v3.2.6 - 2021/09/09

### Improvements

* Use build/release queue user as test "run by" identity (#510)
* Support for SpecFlow v3.8, v3.9 (#531)

### Bug fixes

* Fix: Last test result indicator is not visible in Azure DevOps test suite screen for ADO2019 or earlier (#530)
* Fix: Suite name requires to be unique across all plans even is test plan specified (#513)
* Fix: Test result publishing might not publish results to Test Plans with more than 200 Test Suites (#496)
* Fix: Duplicate test results published with multi-suite publish customization (#502)
* Fix: Test results are not associated with the result when invoked from a release pipeline (#507)

## v3.2.5 - 2021/07/01

### Improvements

* Show license info in "version" command. With the option `--bare`, only the version number is displayed. (See [version](reference/command-line-reference/version-command.md) command.) (#462)
* Show custom value for empty "expected result" using `synchronization/format/emptyExpectedResultValue` setting. (See [format](reference/configuration/configuration-synchronization/configuration-synchronization-format.md) settings.) (#456)
* Allow specifying additional labels for work item links in tags, separated by a semicolon, like `@bug:456:argument_error_for_empty_input`. (See [Linking Work Items using tags](features/common-synchronization-features/linking-work-items-with-tags.md#work-item-tags).) (#460)

### Bug fixes

* Fix: Behave test results not detected when scenario outline placeholder is used in the title (#470)
* Fix: Unable to create link for PRs in another project (#458)
* Fix: Unhandled error "Value cannot be null. (Parameter 'gherkinDialect')" for invalid feature files (#469)

## v3.2.4 - 2021/05/21

### Bug fixes

* Fix: Test statistics are not displayed on stage summary for staged builds (#448)
* Fix: Published tests are not displayed in the "Tests" tab of the build result (see [Use SpecSync from build or release pipeline](important-concepts/synchronizing-test-cases-from-build.md#publishing-test-results-from-build-or-release-pipeline) for details) (#453)

## v3.2.3 - 2021/04/29

### Improvements

* Include interface code documentation for plugin dependencies (#450)

### Bug fixes

* Fix: Error during synchronization for very long project, feature or scenario names (#439)
* Fix: Python Behave skipped status is not mapped, pending is not recognized (#427)
* Fix: Incorrect run completion time published when publishing from a different time zone as the tests run (#445)
* Fix: Attached images are not numbered using test case step number, when useExpectedResult is enabled (#440)
* Fix: Zip packages use incompatible path references for Linux (#437)
* Fix: Synchronizing scenarios with "<" character causes the test case to display incorrectly (#451)

## v3.2.2 - 2021/03/16

### Improvements

* Display more diagnostic information when no test results were published (#425)

### Bug fixes

* Fix: Test step results are published incorrectly when `useExpectedResult` format option is enabled and the scenario has more than 2 Then step (#422)

## v3.2.1 - 2021/03/09

### New features

* [Customization: Automatically link changed Test Cases](features/push-features/customization-automatically-link-changed-test-cases.md) (#412)

### Improvements

* Support SpecSync plugins in .NET Framework runner ([SpecSync.AzureDevOps.Console package](https://www.nuget.org/packages/SpecSync.AzureDevOps.Console)). (#415)
* Support for Jest with jest-cucumber. You need to use `jestCucumberXml` as a publish format. (#395)
* SpecFlow step-level messages are displayed in the test result (#402)
* SpecFlow+ Runner results can be published with Free and Standard editions (#404)
* Support for SpecFlow v3.7 (#414)

### Bug fixes

* Fix: Multi-line step messages are displayed without line separators (#411)
* Fix: Invalid ADO parameter data causes synchronization to stop (#413)

## v3.2.0 - 2021/01/26

### New features

* Publish execution result details as Test Run iterations with step results and the standard output of the tests is also attached to the test result. Step results are published only if the result format supports it (currently for SpecFlow results and Cucumber JSON). (#317, #318)
* Support for new test result formats: Cucumber JSON (works with all Cucumbers), Cucumber.js XML and Cypress JSON files. See all supported formats at in the [Compatibility](reference/compatibility.md#supported-test-result-formats) page. (#349, #341)
* Support publishing test result attachments. Currently supported for TRX and Cucumber JSON result formats. (#122, #360)
* [Customization: Allow reset Test Case state](features/push-features/customization-reset-test-case-state-after-change.md) after change as a separate work item update based on tags. This is useful for example when the Test Case has to be marked as `Ready` if the scenario is tagged with `@ready`, but when work item updates are not allowed in `Ready` state. See [Customization: Reset Test Case state after change](features/push-features/customization-reset-test-case-state-after-change.md) for details. (#378)
* Support .NET 5. The SpecSync [.NET Core Tool](installation/dotnet-core-tool.md) package now also contains a .NET 5 version. The .NET Core 3.1 is still available and can also be used. (#340)

### Improvements

* Support for linking pull requests using tags. To link a pull request, you have to [configure a link synchronization](features/common-synchronization-features/linking-work-items-with-tags.md) with a prefix and with `relationship` set to `Pull Request`. (#388)
* Allow publishing multiple test result files. The test results from the multiple files are going to be merged and published as a single test run. You can list multiple files and directories using the `-r` (`--testResultFile`) option separated by semicolons (`;`). See [Publishing test result files](features/test-result-publishing-features/publishing-test-result-files.md#merging-multiple-test-result-files) for details. (#343)
* Support for SpecFlow v3.5 (#353)
* Support for SpecFlow v3.6 (#391)
* Improved custom update templates: remove description indent, allow scenario/feature source text to be used, allow Rule name and description to be used, can use Gherkin-independent placeholder names (#347, #323)
* Allow environment variables to be used in custom update and custom default template in `{env:ENV_VARIABLE_NAME}` format. (#348)
* Improve error message in case of insufficient PAT authorization scope issue (#124)
* Use folder file source when SpecSync is invoked from a non-dotnet folder and improve error message if it is invoked from the wrong folder. This way you can start using SpecSync for non-dotnet projects without specifying `local` settings in configuration. (#342)
* Allow setting `System.History` field (appears as "Discussion" in Azure DevOps) with custom field updates (#324)
* Use the test configuration of the test suite if there is only one assigned by default for publishing test results. Supported only with Azure DevOps Services, on older versions the configuration has to be specified explicitly with the `-c` (`--testConfiguration`) option or in the config file. (#321)
* Displaying release date, warn if an outdated release is used (#375)
* State change (`synchronization/state/setValueOnChangeTo`) can be limited to scenarios using tag expressions. Can be used together with the Reset Test Case state customization. (#379)
* Improve error reporting of publish-test-results (#350)

### Breaking changes

* Make sub-result publish optional. Because of the improved iteration details publishing (see above), SpecSync will not automatically publish Scenario Outline iterations as sub-results (sub-results are not displayed on Azure DevOps UI, but can be retrieved using the Azure DevOps API). Publishing sub-results can be enabled with `publishTestResults/createSubResults`. (#339)
* Use `condition` instead `skipForTags` in `synchronization/automation` configuration setting. The condition should contain the negated expression of the `skipForTags`, e.g. `not @manual`. (#381)

### Bug Fixes

* Fix: Circular parent config files causes Stack Overflow (#316)
* Fix: Too long examples lines might cause xUnit TRX results unable to load (#361)
* Fix: SDK-style .NET project files with explicitly listed feature file references using wildcards are not supported (#216)
* Fix: SpecSync assemblies have non-real date in PE header timestamp (#368)

## v3.1.2 - 2021/03/09

### Improvements

* Support SpecSync plugins in .NET Framework runner (SpecSync.AzureDevOps.Console package). (#415)
* Improve error message in case of insufficient PAT authorization scope issue (#124)
* Displaying release date, warn if an outdated release is used (#375)

### Bug Fixes

* Fix: Circular parent config files causes Stack Overflow (#316)
* Fix: Invalid ADO parameter data causes synchronization to stop (#413)

## v3.1.1 - 2021/01/05

### Improvements

* Parameters of Scenario Outlines are not visible in published test results if there are only one example. (#320)
* Support localized Azure DevOps processes: set `toolSettings/testCaseWorkItemName` and `toolSettings/testSuiteWorkItemName` configuration settings (#315)

### Bug Fixes

* Fix: Test Cases synchronized from non-SpecFlow project cannot be marked as automated (#345)
* Fix: Test Run automation status is not set on TFS2017 (#352)

## v3.1.0 - 2020/10/26

### New features & improvements

* SpecSync plugin support. See [SpecSync Plugins](features/general-features/specsync-plugins.md) for details. (#110)
* Allow users to enter their credentials interactively in the .NET Core based installations (#298)
* Allow users to save their user name or PAT to the user-specific configuration if they have entered them interactively (#297)
* Non-zero exit code is returned on warnings (#285)
* Force SpecSync tool to exit with zero exit code even in case of warnings using the `--zeroExitCodeForWarnings` flag. (#301)
* Ignore specified Azure DevOps Test Case tags (they are not going to be removed even if there is no corresponding tag on the scenario) See [Customization: Ignoring Test Case Tags](features/push-features/customization-ignoring-test-case-tags.md) for details. (#305)
* Allow ignoring SSL certificate errors for a trusted certificate (#290)
* Show warning when feature files have became invalid after synchronization (#286)
* Diagnostic categories can be specified on command line. The support team might ask you to configure this for diagnosing special integration issues. (#234)
* Allow specifying the SpecSync compatibility version in configuration files. The compatibility version specifies the SpecSync version that the configuration file was created for. The new SpecSync features can be used even if the compatibility version is older than the current version, but if the default values of some settings change in the future, SpecSync will not apply these default unless the compatibility version is updated. (#312)
* Allow to override Run Type setting of the published Test Run (handling automation mismatch error) (#125)

### Breaking changes

* The setting `--baseFolder` becomes obsolete. It can be still used in v3.1 but it is going to be removed in a future release. As an alternative, you can set the working directory of the SpecSync tool. (#311)
* The setting `--diag` becomes obsolete. It can be still used in v3.1 but it is going to be removed in a future release. As an alternative, you should use the equivalent `--verbose` (or `-v`) options. (#309)

### Bug Fixes

All bug fixes od v3.0.3 are also included in this release.

* Fix: Test Case change is not detected if it only contains a link removal and there are no other field changes
* Fix: Make "assemblyBasedExecution" strategy as default (#313)

## v3.0.3 - 2020/10/23

### Improvements

* `--verbose` option also displays diagnostic messages (equivalent to `--diag`). The `--diag` option will become obsolete in v3.1. (#310)
* Display config file being used (#304)
* Suggest checking the [troubleshooting](contact/troubleshooting.md) page on error (#227)

### Bug Fixes

* Fix: Misleading error message when the SpecFlow tools folder is not configured/detected correctly (#300)
* Fix: Should load user-specific config file even if `ignoreParentConfig` is set to true (#306)
* Fix: When [multi-suite-publish customization](features/test-result-publishing-features/customization-publishing-test-results-to-multiple-test-suite.md) is used, the enforcement of specifying a Test Suite is unnecessary (#299)
* Fix: Test cases with more than 200 history item cause synchronization error (#123)
* Fix: Package name is displayed instead of product name in the command line tool (#307)

## v3.0.2 - 2020/09/03

{% hint style="warning" %}
v3.0.1 released earlier on the same day has been deprecated due to a bug for specifying log files. Please use v3.0.2.
{% endhint %}

### Improvements

* Display Support Code: There is a 5-letter support code displayed in the synchronization output for licensed users. Please provide the support code for reporting incidents or license renewal.
* **Publish official SpecSync Docker image**: There are pre-built official Docker images available at [https://hub.docker.com/r/specsolutions/specsync](https://hub.docker.com/r/specsolutions/specsync) that are ready-to-use for synchronizing scenarios or publishing test results. See [Install as Docker image](installation/docker-image.md) for details.
* Improvement: detect unchanged Test Case steps and do not show them in Test Case history
* Set "Run By" field on publish-test-results
* Support SpecFlow v3.4 with Test Suite based execution with Scenario Outline wrappers strategy, using NuGet package: [SpecSync.AzureDevOps.SpecFlow.3-4](https://www.nuget.org/packages/SpecSync.AzureDevOps.SpecFlow.3-4)
* .NET Core binaries are also distributed as [downloadable](downloads.md) zip package

### Bug Fixes

* Fix: Config file settings cannot be overridden from command line if they contain invalid path
* Fix: Configuration error causes exit code 90 (unhandled error) instead of 100
* Fix: SpecSync removes UTF-8 BOM header from the file on link
* Fix: Gherkin Rule keyword is not supported (no scenarios found in file)
* Fix: Invalid Build ID detected from build or release pipeline
* Fix: Unnecessary whitespace at the bottom of DocString (fix applies at the next scenario change unless you perform a push with `--force`)
* Fix: Error when log file does not exist (found in v3.0.1)

## v3.0.0 - 2020/07/24

_Note: The release was formerly planned as v2.2._

{% hint style="info" %}
When upgrading from v2, please check the [Migrating from SpecSync v2 to v3](important-concepts/migrating-from-specsync-v2-to-v3.md) page.
{% endhint %}

### New features & improvements

* .NET Core Support: SpecSync is provided as a .NET Core Tool, the .NET Framework executable is available as NuGet package: [SpecSync.AzureDevOps.Console](https://www.nuget.org/packages/SpecSync.AzureDevOps.Console)
* Using new Azure DevOps API
* Publishing test results to multiple Test Suites (enterprise feature, see [Customization: Publishing test results to multiple Test Suites](features/test-result-publishing-features/customization-publishing-test-results-to-multiple-test-suite.md))
* Allow to specify log file
* init command to setup SpecSync for a new project (see [init command](reference/command-line-reference/init-command.md))
* Hierarchical SpecSync configuration files (see [Hierarchical configuration files](features/general-features/hierarchical-configuration-files.md))
* List known remotes in user-specific configuration file (see [Hierarchical configuration files](features/general-features/hierarchical-configuration-files.md#user-specific-configuration-files))
* Attaching files to test run
* Tag test cases of removed scenarios with `specsync:removed`
* Support for SpecFlow v3.3 with Test Suite based execution with Scenario Outline wrappers strategy, using NuGet package: [SpecSync.AzureDevOps.SpecFlow.3-3](https://www.nuget.org/packages/SpecSync.AzureDevOps.SpecFlow.3-3)
* Map inconclusive test results for publishing (see [Publishing inconclusive test results](features/test-result-publishing-features/publishing-test-result-files.md#publishing-inconclusive-test-results))
* Specify target test suite for publishing on the command line
* Tag expressions support tail wildcards
* Map tags (enterprise feature, see [Customization: Mapping tags](features/push-features/customization-mapping-tags.md))
* Allow specifying test run and test result comments for publishing
* Published test results are automatically connected to the Azure DevOps builds or the build reference can be specified explicitly
* New supported test result formats (besides TRX): Cucumber Java (XML), Python Behave (XML), Cucumber Studio/Hiptest JSON, Python PyTest.BDD (XML), SpecFlow NUnit (XML)
* Simplified licensing: scenario outlines count as one for synchronization limit
* Improved authentication error handling

### Breaking changes

* synchronization/forceUpdate has been removed from configuration (use --force command line option instead)
* Replace --buildServerMode with --disableLocalChanges on the command line and synchronization/enableLocalChanges with synchronization/disableLocalChanges in the config file
* SpecFlow+ Excel support removed
* Linux Mono support has been removed, but SpecSync v3 can be used on Linux and OSX using the .NET Core tool or the [downloadable](http://speclink.me/specsyncdonwloads) native binaries
* The minimum .NET Framework version supported is 4.6.2 (v4.5 is not supported)
* Setting 'useLegacyFeatureFileGenerator' has been deprecated

### Bug Fixes

* Fix: Maximum 1000 test results can be processed with a single call
* Fix: --force does not fix Scenario Outline parameter binding
* Fix: Using shared parameters cause sync error

## v2.1.14 - 2020/07/24

### Improvements

* Support for SpecFlow v3.3 with Test Suite based execution with Scenario Outline wrappers strategy, using NuGet package: [SpecSync.AzureDevOps.SpecFlow.3-3](https://www.nuget.org/packages/SpecSync.AzureDevOps.SpecFlow.3-3)
* Simplified licensing: scenario outlines count as one for synchronization limit

### Bug Fixes

* Fix: --force does not fix Scenario Outline parameter binding
* Fix: Using shared parameters cause sync error

## v2.1.13 - 2020/06/11

### Bug Fixes

* Fix: Parameters are not displayed when test is manually executed using the web-based test runner of ADO (Microsoft.VSTS.TCM.Parameters is not set)

## v2.1.12 - 2020/04/03

### Bug Fixes

* Fix: Publishing xUnit test results cannot find tests with VsTest v16.5.0 or higher
* Fix: Publishing NUnit test results cannot find tests
* Fix: version command should return with exit code 0

## v2.1.11 - 2020/02/23

### Improvements

* Improved test case result comment when publishing test results of scenario outlines

### Bug Fixes

* Fix: DataTable headers missing when scenario is pulled
* Fix: DataTable is not pulled properly when `syncDataTableAsText` formatting option is enabled
* Fix: DocString is not pulled properly
* 3rd party open-source licenses (`third-party-licenses.txt`) added to the package. The information is also available from the [Licensing](licensing.md) page.&#x20;

## v2.1.10 - 2019/11/01

### Improvements

* Security protocol can be configured for HTTPS (e.g. to avoid using TLS 1.0). To override the default configuration set the `remote/securityProtocol` setting in the configuration file to one of the following values: `Ssl3`, `Tls`, `Tls11`, `Tls12`
* Support for SpecFlow v3.1 (via NuGet package [`SpecSync.AzureDevOps.SpecFlow.3-1`](https://www.nuget.org/packages/SpecSync.AzureDevOps.SpecFlow.3-1)).&#x20;

### Bug Fixes

* Fix: Test result remains in "In Progress" state when publishing test results

## v2.1.9 - 2019/08/22

### Bug Fixes

* Fix: useTestCaseData value for setting specFlow/scenarioOutlineAutomationWrappers causes "Invalid Test Case ID" error
* Fix: Incorrect detection of PAT when specified as %foo% in config file
* Fix: Incorrect display of user name when specified as %foo% and asks for password

## v2.1.8 - 2019/08/15

### Bug Fixes

* Fix: execution on Linux using the dependency-bundled executable fails with TypeInitializationException

### Improvements:

* Native library `libMonoPosixHelper.so` does not need to be copied to the project folder for native Linux execution

## v2.1.7 - 2019/08/12

### Bug Fixes

* Fix: execution on Linux using the dependency-bundled executable fails with assembly load error

## v2.1.6 - 2019/07/08

### Bug Fixes

* Fix: `specsync4azuredevops.cmd` does not tolerate spaces in project path
* Fix: default feature file language is not picked up from SpecFlow config

## v2.1.5 - 2019/06/14

### Bug Fixes

* Fix: Error when synchronizing SpecFlow V3 projects that use specflow.json config file

## v2.1.4 - 2019/05/29

### Bug Fixes

* Fix: Publish tests does not find SpecFlow+ Runner test results without feature name in the test definition name
* Fix: Publish tests reports the test as "Not Executed" when there are skipped test targets in a SpecFlow+ Runner test result
* Fix: Collect all test result for scenarios executed with SpecFlow+ Runner using multiple test targets
* Fix: Include example name for the test result error, stack trace and comment fields created for scenario outlines
* Show license expiration date and remaining time warning

## v2.1.3 - 2019/05/08

### Bug Fixes

* Fix: Publish test results fails for TFS2018

## v2.1.2 - 2019/04/08

### Bug Fixes

* Fix "Error during generation" error when linking new scenarios

## v2.1.1 - 2019/04/04

### Bug Fixes

* Fix generation with SpecFlow.Tools.MsBuild.Generation for SpecFlow 2.4

## v2.1.0 - 2019/04/02

### Breaking changes

* When synchronizing automated test cases, a test execution strategy has to be configured in the [synchronization/automation](reference/configuration/configuration-synchronization/configuration-synchronization-automation.md) section of the configuration file. To read more about test execution strategies see the [Synchronizing automated test cases](important-concepts/synchronizing-automated-test-cases.md) article.

### New features

* Support for SpecFlow v3 both .Net Framework and .Net Core
* New test execution strategy for automated test cases: Assembly based execution
  * Supports all test runners (MsTest, xUnit, NUnit, SpecFlow+ Runner)
  * Supports both .NET and .NET Core
  * Available for TFS 2017 or newer
  * See more in the [Synchronizing automated test cases](important-concepts/synchronizing-automated-test-cases.md) article
* Updated "Test Suite Based execution strategy": now supports MsTest, xUnit and NUnit in Azure DevOps (for MsTest, Scenario Outline wrapper and SpecSync SpecFlow plugin is needed.
  * See more in the [Synchronizing automated test cases](important-concepts/synchronizing-automated-test-cases.md) article
* Allow overriding base folder using the `--baseFolder` console option
* Allow specifying Test Plan ID to be able to improve Test Suite loading time
* Automatically create Test Suite if does not exists

### Bug Fixes

* Fix displaying load project source in console
* Fix: Invalid "\_MsTest" suffix might be added to the automated test class name
* Better error reporting for project load error
* Fix project URL error message
* Fix changing link type

## v2.0.0 - 2019/01/14

### Breaking changes

* The product has been renamed to SpecSync for Azure DevOps to be conform with the updated product name of Microsoft TFS/VSTS
* The NuGet package has been renamed and split:
  * `SpecSync.AzureDevOps` - contains the synchronization tool
  * `SpecSync.AzureDevOps.SpecFlow.2-3`, `SpecSync.AzureDevOps.SpecFlow.2-4`, etc. - contains the SpecFlow plugin required to [synchronized automated test cases](important-concepts/synchronizing-automated-test-cases.md) for a particular SpecFlow version (eg. v2.3, v2.4).
  * `SpecSync.MTM` - obsolete package, will load SpecSync.AzureDevOps, SpecSync.AzureDevOps.SpecFlowPlugin has to be added manually if necessary
* The synchronization tool has been renamed from `SpecSync4MTM` to `SpecSync4AzureDevOps`
* The configuration of the synchronization has been moved to a configuration file (although a few options can be overridden from the command line). By default the config file should be named as `specsync.json`. See configuration options and samples at [Configuration](reference/configuration/).
* The two-way synchronization has been reworked to be able to support a more stable development process. The synchronization has been split into two independent actions: _push_ and _pull_. The new process should follow a pull-merge/resolve-verify-push model. See [Two-way synchronization](features/pull-features/two-way-synchronization.md) for details.
* Tag filtering has been split to _tag filter_ and _tags scope_. Tag filter has to be specified on the command line (`--tagFilter`), while tag scope has to be specified in the config file (`local/tags`). See [Filters and scopes](important-concepts/filters-and-scopes.md) for details.
* If a scenario was linked to a test case that does not exist, SpecSync reports this as an error (in v1 it linked to a new scenario and replaced the wrong tag)
* The 'state' field of the test case is set to the value provided in configuration `synchronization/state/setValueOnChangeTo` not only when the test case changed (v1) but also when the test case is created.
* The default way of generating test method wrappers has changed to `iterateThroughExamples`. See [`specFlow` configuration](reference/configuration/configuration-specflow.md) for details.

### New features

* Support for specifying test case field default values and custom field updates. See [`customizations` configuration](reference/configuration/configuration-customizations.md).
* Support for skipping the `Scenario` or `Scenario Outline` prefixes in test case title. See [`synchronization` configuration](reference/configuration/configuration-synchronization/).
* Test case tag prefix name (by default `tc`) can be configured. See [`synchronization/format` configuration](reference/configuration/configuration-synchronization/configuration-synchronization-format.md).
* Support for ignoring test case steps with a specific prefix text. See [`customizations` configuration](reference/configuration/configuration-customizations.md).
* Support for synchronization of scenarios on a feature branch. See [`customizations` configuration](reference/configuration/configuration-customizations.md).
* Support for folder-based synchronization. See [`local` configuration](reference/configuration/configuration-local.md).
* SpecSync can track format configuration changes and automatically re-sync scenarios without the `--force` option.
* More readable sync trace output.&#x20;

## v1.6.0 - 2018/07/25

* Support for SpecFlow v2.3.\*
* Support for synchronizing scenarios from a branch to a temporary set of test cases (see `--branchTagPrefix` option, experimental)
* Fix: Generating feature-file code-behind files fail with the SpecFlow Visual Studio integration v2017.1.11 or later.
* Fix: Do not remove filtered out test cases from the test suite.
* Fix: Invalid tools version error when back-syncing new test cases.
* Fix: Handle `&nbsp;` in test cases.
* Fix: Use proper space encoding in the URL specified in the `@tfs` tag.
* Fix: Do not add `@tfs` tag to feature if otherwise not changed (scenarios skipped)
* Fix: Space handling in TFS collection name.
* Fix: Missing parameter in parametrized test case if the Scenario Outline placeholder is used in a DataTable.
* Improved error handling

## v1.5.0 - 2018/04/11

* Support for SpecFlow v2.3.1 (for SpecFlow v2.2.1, please use v1.4.\*)

## v1.4.1 - 2018/04/10

* Fix: ArgumentOutOfRangeException when synchronizing empty feature file
* Fix: Do not count skipped scenarios in summary
* Improved error handling

## v1.4.0 - 2018/03/20

* Use new TFS REST API (supports TFS 2015+)
* Support synchronizing non-SpecFlow projects with the `--listFilePath` option. See page [Using SpecSync with Cucumber](http://speclink.me/specsynccucumber) for details.
* Allow executing the synchronization from OSX and Linux systems. See page [Using SpecSync on OSX/Linux](http://speclink.me/specsyncosxlinux) for details.
* Filter scenarios to be synchronized with [tag expressions](https://docs.cucumber.io/tag-expressions/) using the `--tags` option.
* `--skipAutomation` supports [tag expressions](https://docs.cucumber.io/tag-expressions/).
* Improved test case formatting options (see [Configuring the format of the synchronized test cases](features/push-features/configuring-the-format-of-the-synchronized-test-cases.md))
  * Synchronize `Then` steps to the _expected results_ column of the test case steps by specifying the `--useExpectedResult` option.
  * Synchronize data tables as plain text by specifying the `--syncDataTableAsText` option.
  * Skip adding the _Background:_ prefix for background steps by specifying the `--doNotPrefixBackgroundSteps` option.
  * Force synchronization even if the scenario text did not change by specifying the `--force` option. This is useful after changing formatting options like `--useExpectedResult`.
* Better handling invalid Gherkin files.
* Generate `@tc:123` tags on new lines (before the first existing tag line).
* Update existing (invalid or incomplete) `@tc:` tags instead of generating a new one.

## v1.3.2 - 2017/10/31

* Fix SpecFlow+ Runner compatibility issue for Scenario Outlines

## v1.3.1 - 2017/10/10

* Feature: Specify an area or an iteration for the newly created test cases using `--areaPathForCreate` and `--iterationPathForCreate`options See [Add new test cases to an Area or an Iteration](https://gasparnagy.gitbooks.io/specsync-for-mtm-documentation/content/add-new-test-cases-to-an-area-or-an-iteration.html) for details.&#x20;
* Upgrade to SpecFlow 2.2.1.
* Fix project loading issues.
* Fix SpecFlow+ Runner integration issue.
* Fix Newtonssoft.Json loading issue.
* Fix IndexOutOfBounds error when synchronizing data tables with empty cells.

## v1.2.0 - 2017/03/31

* Support synchronizing '@us:123' style tags to links between the test case and other work items. The tag prefixes and the link type can be specified. See [Linking work items using tags](features/common-synchronization-features/linking-work-items-with-tags.md) for details.
* Small fixes in Scenario Outline table handling
* Fix compatibility issue for running synchronized test cases using the TFS lab environment.
* Add application icon

## v1.1.6 - 2016/08/11  <a href="v1-1-0" id="v1-1-0"></a>

* Fix authorization problem with VSTS for Alternate authentication credentials and Personal access tokens
* Set test case tags to the tags of the scenario
* Only update changed test cases
* Synchronize automation does not need to have a compiled test assembly
* Feature files are automatically regenerated if changed
* Optionally set test case state to a configured value when test case is updated
* Synchronize test cases to test suites
* Support for SpecFlow+ Excel examples
* More robust error handling
* Two-way synchronization (beta)

## v1.0.1 - 2016/03/17  <a href="v1-0-1" id="v1-0-1"></a>

* Only display the exception messages by default, you can use the `--verbose` option to get the stack trace.
* The tool returns with non-zero exit code if there was an error.
* The ESC key can be used to cancel password entering.
* Allow specifying environment variables in user name.
* Fix tag placing issue.
* Allow more authentication options (sign-in prompts, domain account)
* Tested with TFS 2013.

## v1.0.0 - 2016/02/24  <a href="v1-0-0" id="v1-0-0"></a>

Initial version used in the demo video in the [intro post](http://speclink.me/spsintro).
