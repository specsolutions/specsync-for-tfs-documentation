# Using SpecSync with Cypress

Cypress is a popular end to end testing tool that supports BDD-style testing as well.

By configuring Cypress with the `cypress-cucumber-preprocessor`, you can express your tests as BDD scenarios using the Gherkin format. As the format of the feature files are compatible with SpecSync, you can use SpecSync to synchronize them with Azure DevOps Test Cases. The installation and configuration options are the same as with [using-specsync-with-cucumber.md](using-specsync-with-cucumber.md "mention").

There are two versions of the `cypress-cucumber-preprocessor` plugin. This guide focuses on the newer, [@badeball/cypress-cucumber-preprocessor](https://www.npmjs.com/package/@badeball/cypress-cucumber-preprocessor) package, but the section ["Use SpecSync with older version of `cypress-cucumber-preprocessor`"](#old-preprocessor) below shows the differences if a project uses the older, unmaintained [cypress-cucumber-preprocessor](https://www.npmjs.com/package/cypress-cucumber-preprocessor) package.

Publishing test results from Cypress requires a few simple configuration steps.

Perform the following steps to generate the test results correctly with SpecSync.

### Step 1: Check cypress-cucumber-preprocessor configuration

In order to publish the test results from Cypress, the Cypress result files (XML files in the results folder) cannot be used, but the _cypress-cucumber-preprocessor_ has to be configured to generate JSON test result files, that also contain step-level results and screenshots as well. This can be done by adding the following configuration section to the `package.json` configuration file.

{% code title="package.json" %}
```json
  "cypress-cucumber-preprocessor": {
    "nonGlobalStepDefinitions": false,
    "step_definitions": "cypress/support/step_definitions/",
    "json": {
      "enabled": true,
      "output": "cypress/cucumber-json/cucumber-report.json"
    }
  },

```
{% endcode %}

The relevant part of the configuration above is the `json` setting for the results, as it instructs Cypress to generate a Cucumber-JSON output file for the test execution. In the example above, Cypress is configured to generate this file to `cypress/cucumber-json/cucumber-report.json`.

### Step 2: Run Cypress tests as usual

You can run the Cypress tests now. The test results settings are not relevant for the SpecSync integration, because SpecSync will anyway use the JSON files generated in the folder configured in Step 1.

A simple command invocation is enough:

```
npx cypress run
```

or

```
cypress run
```

### Step 3: Publish test results with SpecSync

If the scenarios have been synchronized earlier already with SpecSync, you will be able to publish the test results from the folder configured in Step 1 using the SpecSync publish-test-results command. For that

* you have to specify the test result file
* you have to use `cypressCucumberJson` as format specifier

The following example publishes the test results from the `cypress/cucumber-json/cucumber-report.json` file.

```
dotnet specsync publish-test-results --testResultFormat cypressCucumberJson --testResult cypress/cucumber-json/cucumber-report.json
```

The same can be achieved with the short option forms:

```
dotnet specsync publish-test-results -f cypressCucumberJson -r cypress/cucumber-json/cucumber-report.json
```

With the configuration and the steps above, the test results are now published to Azure DevOps.

## Use SpecSync with older version of `cypress-cucumber-preprocessor` <a href="old-preprocessor" id="old-preprocessor"></a>

In case you use the older (unmaintained) version of the package, through the [cypress-cucumber-preprocessor](https://www.npmjs.com/package/cypress-cucumber-preprocessor) package, the configuration steps are slightly different.

The configuration in the `package.json` should use the `cucumberJson` setting instead of the `json` setting like in the example below.

{% code title="package.json" %}
```json
  "cypress-cucumber-preprocessor": {
    "nonGlobalStepDefinitions": false,
    "step_definitions": "cypress/support/step_definitions/",
    "cucumberJson": {
      "generate": true,
      "outputFolder": "cypress/cucumber-json",
      "filePrefix": "",
      "fileSuffix": ".cucumber"
    }
  },

```
{% endcode %}


In this case the `outputFolder` setting specifies a folder. As there will be multiple report files generated to this folder, when publishing the results with SpecSync, you have to *specify the folder as a result instead of an individual result file*.

```
dotnet specsync publish-test-results -f cypressCucumberJson -r cypress/cucumber-json
```
