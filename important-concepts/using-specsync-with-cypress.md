# Using SpecSync with Cypress

Cypress is a popular end to end testing tool that supports BDD-style testing as well.

By configuring Cypress with the [cypress-cucumber-preprocessor](https://www.npmjs.com/package/cypress-cucumber-preprocessor), you can express your tests as BDD scenarios using the Gherkin format. As the format of the feature files are compatible with SpecSync, you can use SpecSync to synchronize them with Azure DevOps Test Cases. The installation and configuration options are the same as with [using-specsync-with-cucumber.md](using-specsync-with-cucumber.md "mention").

Publishing test results from Cypress is a little bit different from the publishing process with other BDD tools as it does not generate a single test result file.&#x20;

Perform the following steps to generate the test results correctly with SpecSync.

### Step 1: Check cypress-cucumber-preprocessor configuration

In order to publish the test results from Cypress, the Cypress result files (XML files in the results folder) cannot be used, but the _cypress-cucumber-preprocessor_ has to be configured to generate JSON test result files. This can be done by adding the following configuration section to the `package.json` configuration file.

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

The relevant part of the configuration above is the `cucumberJson` setting for the results, as it instructs Cypress to generate Cucumber-JSON output files for the test execution. In the example above, Cypress is configured to generate these files to the `cypress/cucumber-json` folder. (This is the recommended default for the cypress-cucumber-preprocessor, see "Output" section in the [documentation](https://www.npmjs.com/package/cypress-cucumber-preprocessor).)

### Step 2: Run Cypress tests as usual

You can run the Cypress tests now. The test results settings are not relevant for the SpecSync integration, because SpecSync will anyway use the JSON files generated in the folder configured in Step 1.

A simple command invocation is enough:

```
cypress run
```

### Step 3: Publish test results with SpecSync

If the scenarios have been synchronized earlier already with SpecSync, you will be able to publish the test results from the folder configured in Step 1 using the SpecSync publish-test-results command. For that&#x20;

* you have to specify the folder name as test result file (as there are multiple files there)
* you have to use `cypressCucumberJson` as format specifier

The following example publishes the test results from the `cypress/cucumber-json` folder.

```
dotnet specsync publish-test-results --testResultFileFormat cypressCucumberJson --testResultFile cypress/cucumber-json
```

The same can be achieved with the short option forms:

```
dotnet specsync publish-test-results -f cypressCucumberJson -r cypress/cucumber-json
```

With the configuration and the steps above, the test results are now published to Azure DevOps.
